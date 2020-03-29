---
title: "Document Your Process Builder"
date: 2020-03-27T00:49:35+09:00
image: "posts/img/flowdoc.png"
draft: false
---
We often create various documents for deliverables, review, taking over, just an explanation to teammates, and so on. If the document is based on your Salesforce environment, you may create it automatically from the org to avoid maintaining both.

For example, the followings are popular tools to export Salesforce settings including object fields definition. 
- [Salesforce DevTools](https://chrome.google.com/webstore/detail/salesforce-devtools/ehgmhinnhggigkogkbhnbodhbfjgncjf)
- [docforce2](https://github.com/nyasba/docforce2)

And some companies may have their own tools internally.

So, how about Lightning Flow? [This idea](https://success.salesforce.com/ideaView?id=08730000000DmiPAAS) has more than 3k points but the metadata of lightning flow is complex and I cannot find any open source tools that supports it.

Not found? Just make it! As a prototype, I created the following tools to document process builder setting.

{{< figure src="https://github.com/shunkosa/sfdx-flowdoc-plugin/raw/master/img/screenshot.png" width="100%" title="" >}}

* [Flowdoc Web App](https://flowdoc.herokuapp.com)
* [Flowdoc Salesforce CLI Extension](https://www.github.com/shunkosa/sfdx-flowdoc-plugin)

## Web App
Connect your org and click the download icon of the flow you want to export. PDF file will open in another browser tab. Currently this tool supports trigger-based process builder only. (And unfortunately it still doesn't support all the process builder settings.)

![](../img/flowdoc_screenshot.png)

## CLI Extension
Install the plugin
```
sfdx plugins:install sfdx-flowdoc-plugin
```

and then run,
```
sfdx flowdoc:pdf:generate YOUR_PROCESS_API_NAME
```

## Behind the Scenes
This tool simply parses [Flow metadata](https://developer.salesforce.com/docs/atlas.en-us.api_meta.meta/api_meta/meta_visual_workflow.htm) and construct a object to generate pdf by [pdfmake](http://pdfmake.org/#/). For example, a criteria element in process builder is represented as `<decisions>` element in medatada.

![](../img/process_metadata_example.png)

But `<decisions>` represents not only criteria in process, but also evaluation of scheduled action, and of course decision in flow builder. Now I write code only for displaying process builder settings, so further studies on flow metadata are needed to make this more abstract. 

## Next Step
I'm working on the rest of process builder settings and flow builder settings. Your feedback is always welcomed!