---
title: UCF Composition 2 Pure Data resources
---

I will upload example start/end files as they are relevant to class activities. They will be listed below under "Example Patches".  

## Resources

- [puredata.info](http://puredata.info) - This is about as close to an official website as there is for Pd. (The real [official site](http://msp.ucsd.edu/) is not very useful for much beyond downloading the tool.)
- [Pd forums](https://forum.pdpatchrepo.info/) - There are nice people here. Be nice and ask specific questions if you are looking for help. Or, you can just search their very useful archives. 
- [Pure Data Tutorials](https://www.youtube.com/playlist?list=PL12DC9A161D8DC5DC) by Rafael Hernandez - Hernandez offers some succinct introductory videos that are a good place to start. When he refers to "Pure Data extended", note that it no longer is supported. The modern equivalent is Pd-l2Ork, or [Purr Data](https://github.com/jonwwilkes/purr-data/releases). If you're using Pd on the iMacs in the lab, they're running "vanilla" Pd. Everything he demonstrates here will work fine in both places. Also, you don't necessarily need to watch them in sequence. Once you get through the basics, feel free to jump around if you see a topic that interests you. 

## Outlines

{% for page in site.pages %}
	{% if page.url contains 'outlines/' %}
- [{{ page.title }}]({{ site.baseurl }}{{ page.url }})
	{% endif %}
{% endfor %}

## Example Patches

{% for file in site.static_files %}
	{% if file.path contains '.pd' and file.path contains 'examples/' %}
- [{{ file.name }}]({{ site.baseurl }}{{ file.path }})
	{% endif %}
{% endfor %}
