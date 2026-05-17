---
title: "Why Tesla FSD is vision-only: the real engineering bet"
date: 2026-05-16
categories: ai technology architecture
layout: post
---

I have been arguing with myself about Tesla's FSD architecture
for a while. Every time I bring this up with someone, the
conversation goes the same way: "Tesla uses cameras only because
humans drive with eyes only." And every time I want to push back.
That argument is the marketing version, not the engineering one.

Here is my objection in plain terms. If I, as a human, could have
a radar wired into my brain alongside my eyes, I would install it
tomorrow. It is not that humans don't *need* radar. It is that
humans *can't have* radar. The biological constraint is not a
design principle, it is just a constraint. Anyone arguing
otherwise is doing post-hoc rationalization.

So if "we mimic humans" is the bullshit answer, what is the real
one? After thinking about it for a while, I'm convinced the
actual argument has several layers, and the deepest one is
genuinely interesting --- and contested.

## The proximate reason: sensor contention

The immediate engineering reason Tesla pulled radar in 2021 is
what they call sensor contention. When your radar says "stopped
object 80m ahead" and your camera says "open road," some piece of
code has to decide which one wins. That arbitration logic is
where bugs and ambiguity live.

Tesla's old continental-supplied radar was particularly bad at
this. Conventional automotive radar has fundamental weaknesses
with stationary objects that can't produce a Doppler shift, with
thin cross-sections, and with low-reflectivity targets. The
result was the infamous phantom braking: overhead signs, manhole
covers, parked emergency vehicles getting flagged as obstacles,
triggering AEB on the freeway. When the radar misclassifies an
innocuous object as a hazard, the system fires the brakes for no
apparent reason.

So you have a sensor that is noisy in ways your camera isn't, and
you have to decide a trust hierarchy. Tesla's empirical finding
was that as their vision stack got better, the cases where radar
was *right and vision was wrong* became smaller than the cases
where *radar was wrong and vision was right*. At that crossover
point, the radar is net negative.

But that is just the proximate reason. If that were all there was
to it, you would say "wait for better radar." Imaging radar
(4D radar from Arbe, Continental, Mobileye, and others) largely
solves the phantom-braking problem. Tesla actually shipped HW4
Model S/X with high-definition radar hardware in 2023, and then
*never activated it* for use in FSD. So why didn't they re-enable
it once they had better hardware? That is where the deep argument
lives.

## The deep reason: the Bitter Lesson, applied to autonomy

Rich Sutton's [Bitter Lesson][bitter-lesson] --- which you
probably know from the LLM world --- says that general methods
that scale with compute and data beat hand-engineered,
domain-specific approaches every time, eventually. The history of
ML keeps validating this. Feature engineering lost to deep
learning. Grammar-based NLP lost to transformers. AlphaGo's
hand-crafted heuristics lost to AlphaZero's self-play.

Sensor fusion, as classically practiced, is the hand-engineered
approach to perception. Kalman filters, occupancy grids,
probabilistic data association, hand-tuned per-sensor noise
models, hand-tuned arbitration rules. It is the equivalent of
pre-deep-learning computer vision: lots of human-injected priors,
brittle at the edges, expensive to extend.

Vision plus end-to-end neural networks is the general method.
Photons in, controls out, let gradient descent figure out the
representations. The bet is that this approach has unbounded
headroom --- feed it more data and more compute and it keeps
getting better --- while the sensor-fusion approach has a ceiling
determined by how cleverly humans can hand-engineer the fusion.

If you believe the Bitter Lesson applies here, then adding
another modality is not free. Every modality you add is more
hand-engineering at the fusion seam, more places where the
general method gets contaminated by the specialized one. You
would only add a modality if it provides something vision
fundamentally can't recover, *and* if the marginal gain exceeds
the integration tax. Tesla's bet is that for cars on roads, that
bar is never met by radar.

## The asymmetry of necessity vs. sufficiency

This is the part of the human-comparison argument that survives
once you strip out the biology mysticism. Forget "humans drive
with eyes so we should too." The actual structural argument is:

Vision is *necessary*. You cannot drive without reading signs,
lane markings, brake lights, turn signals, traffic lights, hand
gestures from a police officer, the body language of a pedestrian
about to step off a curb. None of those are radar-detectable. So
no matter what other sensors you have, you must solve vision to a
level where it is safe.

Now: once you have solved vision to that level, what does radar
add? It adds a redundant channel for the subset of cases where
vision is uncertain about depth or velocity --- primarily in
degraded visual conditions like fog, heavy rain, direct sun
glare, lens occlusion. That is real, but it is a narrow slice of
the operational design domain, and it comes with the integration
tax.

My own intuition --- "if I could install a brain-radar I would"
--- quietly assumes the integration is free. In a biological
brain with a hypothetical radar input that comes pre-fused with
vision in some magic way, sure. But in an engineered system, the
integration is not free, and the tax compounds against the Bitter
Lesson trend.

## The data and architecture flywheel

This is where the strategic logic gets interesting. Tesla has
millions of cars on the road, each one a sensor platform. The
training data pipeline is built around the assumption that every
car produces comparable data. The moment you split the fleet by
sensor configuration, you split your training data, your
validation suite, and your release process. Either you maintain
two stacks forever, or you carry the lowest-common-denominator
hardware on every vehicle.

This matters especially for end-to-end neural networks, which is
where FSD went with V12 and later. End-to-end multimodal
architectures exist --- VLMs do this with text and images --- but
adding a sparse, low-bandwidth, asynchronous modality like radar
to a dense video stream is architecturally awkward. Radar returns
have a totally different structure than image pixels: irregular
point clouds at low rates, with measurements that are
physics-dependent on target geometry. You can do it (Waymo does),
but it complicates training, complicates the loss surface, and
introduces failure modes where the network learns to over-rely on
whichever modality is easier to fit.

Tesla's architecture instead reconstructs a dense 3D occupancy
volume from the eight cameras using a neural network. They
effectively *synthesize* a lidar-like representation from vision
--- the "pseudo-lidar" or [occupancy network][occupancy] approach.
If you believe neural networks can extract depth and velocity
from multi-camera video at sufficient fidelity --- and the demos
suggest they can --- then the geometric information radar would
have provided is already available in the vision pipeline, just
through a different mechanism.

## The contrarian view, which is not crazy

I should be fair: this is a contested bet, not a settled
question. Waymo founder John Krafcik recently stated that Tesla's
camera-only approach, along with its limited resolution and wide
field of view, gives the system myopia that makes it worse than a
human driver. The argument against Tesla's path goes like this.

The Bitter Lesson applies when you have effectively unlimited
compute and data. In safety-critical real-time perception with
hard latency budgets and long-tail failure modes --- a kid
running into the street, debris on the highway at night --- you
do not get to wait for the next scaling jump. You need a system
that fails gracefully *now*. Redundant modalities provide that
graceful failure. A camera blinded by sun glare is a single point
of failure; a radar in parallel is not subject to the same
failure mode. That is the classic safety-engineering argument,
and it is not wrong.

There is also an information-theoretic argument: radar gives you
direct measurements of range-rate (velocity) that vision can only
infer. For a fast-closing object at long range in poor
visibility, that direct measurement is qualitatively different
from the inferred one. Whether that difference matters at the
margin depends on how good the inference gets.

Waymo's empirical track record in geo-fenced operations is the
strongest counter-evidence to Tesla's position. Tesla's strongest
counter-evidence is the rate of FSD capability improvement and
the fact that they have kept pulling sensors out (radar,
ultrasonics) without their safety metrics degrading.

## What I think the deep idea actually is

The deep idea is not "humans don't need radar." It is a *scaling
hypothesis*: the long-run winner in autonomy is the architecture
that benefits most from compute, data, and model scale, and
adding modalities adds engineering surface area that compounds
against you over time. It is the same hypothesis that led OpenAI
to bet on autoregressive transformers at scale instead of more
sophisticated symbolic-neural hybrids.

That hypothesis could be wrong. It is an empirical question that
will be answered by which approach actually crosses the threshold
to robust L4 driving first, in which conditions, and at what unit
economics. Tesla is betting that vertical integration plus scale
plus the Bitter Lesson wins. Waymo is betting that more sensors
plus more careful engineering plus geographic constraint wins.
They might both be right in different niches.

The part of Musk's pitch I started this post objecting to ---
"humans only have eyes" --- is rhetorical cover for the actual
technical bet, which is this: scaling vision-only end-to-end
neural networks beats hand-engineered multi-modal fusion in the
limit, and we would rather position for the limit than optimize
for today.

That is a bet I can argue with on the merits. It is not a bet
about biology. It is a bet about which side of the Bitter Lesson
you want to be standing on when the scaling curve crosses your
problem.

[bitter-lesson]: http://www.incompleteideas.net/IncIdeas/BitterLesson.html
[occupancy]: https://www.thinkautonomous.ai/blog/occupancy-networks/
