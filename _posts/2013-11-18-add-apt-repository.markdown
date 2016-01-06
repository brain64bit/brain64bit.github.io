---
layout: post
title: "add-apt-repository, server certificate verification failed"
date: 2013-11-18 16:56:50 +0700
comments: true
categories: [ubuntu, server, linux, apt]
---
I've got encountered problem when try to add new repository to ubuntu 12.04 using apt :

    Traceback (most recent call last):
    	File "/usr/bin/apt-add-repository", line 125, in <module>
    	ppa_info = get_ppa_info_from_lp(user, ppa_name)
    File "/usr/lib/python2.7/dist-packages/softwareproperties/ppa.py", line 84, in get_ppa_info_from_lp
    	curl.perform()
    pycurl.error: (60, 'server certificate verification failed. CAfile: /etc/ssl/certs/ca-certificates.crt CRLfile: none')
<!-- MORE -->
this error happen since ppa.py depend on pycurl, curl trying to verify peer and host with curl.setopt(pycurl.SSL_VERIFYPEER, 1) and curl.setopt(pycurl.SSL_VERIFYHOST, 2):

	def get_ppa_info_from_lp(owner_name, ppa_name):
	lp_url = LAUNCHPAD_PPA_API % (owner_name, ppa_name)
		# we ask for a JSON structure from lp_page, we could use
		# simplejson, but the format is simple enough for the regexp
		callback = CurlCallback()
		curl = pycurl.Curl()

		curl.setopt(pycurl.SSL_VERIFYPEER, 1)
		curl.setopt(pycurl.SSL_VERIFYHOST, 2)

		curl.setopt(pycurl.WRITEFUNCTION, callback.body_callback)
		# only useful for testing
		if LAUNCHPAD_PPA_CERT:
			curl.setopt(pycurl.CAINFO, LAUNCHPAD_PPA_CERT)
		curl.setopt(pycurl.URL, str(lp_url))
		curl.setopt(pycurl.HTTPHEADER, ["Accept: application/json"])
		curl.perform()
		curl.close()
		lp_page = callback.contents
		return json.loads(lp_page) 
from [http://curl.haxx.se/libcurl/c/curl_easy_setopt.html#CURLOPTSSLVERIFYHOST](http://curl.haxx.se/libcurl/c/curl_easy_setopt.html#CURLOPTSSLVERIFYHOST) you can bypass this verification using pass a long set to 0 :

	...
	curl.setopt(pycurl.SSL_VERIFYPEER, 0)
	curl.setopt(pycurl.SSL_VERIFYHOST, 0)
	...