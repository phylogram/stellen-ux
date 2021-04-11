---
title: About
---
# About

The main concern for developing this API was time – due to a full schedule –, and a presentable output of course.

So I choose to write a [nuxt app](https://nuxtjs.org/), with [bootstrap-vue](https://bootstrap-vue.org/) which makes it easy to
implement the [fundamental styles](https://fundament.acdh.oeaw.ac.at). Their 
[table component](https://bootstrap-vue.org/docs/components/table) was used for the result list.

The API-Requests are done with [axios](https://github.com/axios/axios). The CMS-Part (like this), and the search is 
implemented using [nuxt-content](https://content.nuxtjs.org/). Routing is done with [Vue Router](https://router.vuejs.org/).


## Caveats

There is no testing and no ci/cd. Parts of this app are not dry – for example this one here. `pages/about.vue`, and 
`pages/feedback.vue` are almost the same. The integration of th bootstrap table with the bootstrap form is not done
elegantly.

And some additional styling would be cool –

