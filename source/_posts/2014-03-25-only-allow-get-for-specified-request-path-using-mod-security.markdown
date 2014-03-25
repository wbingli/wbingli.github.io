---
layout: post
title: "Only allow GET for specified request path using mod security"
date: 2014-03-25 17:47:06 +1300
comments: true
categories: ops
tags: mod_security apache web
---
## Mod Security
Recently we want to directly use couchdb REST API. But only expose GET REST API so the data could be safe for the public. 

A simple mod security rule do this trick:

	SecRule REQUEST_URI "/database" "chain,log,deny,status:403,phase:2,id:1234567010"
	SecRule REQUEST_METHOD "!@rx ^(?:GET)$" 

## Two layers application - AngularJS + CouchDB
Speaking to this mod security, we are actually working some simple two layers application, only front-end(AngularJS) and datastore(Couchdb). 

This simple stack suits for some applications, which only load data from backend.And backend data would be updated by application operator.Those applications could be FAQ or News website etc. We use CouchDB because it has very good REST API and Web UI to manage the data.

So no application layer, :-)

