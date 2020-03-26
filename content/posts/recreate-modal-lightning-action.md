---
title: "Recreate Modal Component for force:lightningQuickActionWithoutHeader"
date: 2017-12-12T21:18:40+09:00
draft: false
---
## Intro
Lightning component action is a handy option to create custom function called from record detail page. We can launch a lightning component from a custom action by just implementing either ``force:lightningQuickAction`` or ``force:lightningQuickActionWithoutHeader``. However, lightning component action has serious user interface problems. I will introduce them and propose a workaround.

## Problems
A component implemented force:lightningQuickAction is shown on the auto-created modal and its header and footer are outside of the component. So we can neither customize the header text nor put an another button next to the cancel button on the footer.
![](../img/20171212181307.png)

It's a natural to think about recreating everything in a component implemented force:lightningQuickActionWithoutHeader. However, it has another problem. Extra margin is generated.
![](../img/20171212181318.png)
## Workaround
**Notice: The following workaround is not compatible with Salesforce mobile and not available for API version 42.0 or later.**

We can overwrite the style of the auto-created modal by inline CSS to disable extra margin. Another possibility is calling a custom modal from lightning:button tag but it requires you to recreate a highlight panel and it would be a tough work.
```
.cuf-content{
    padding:0 !important;
}
.slds-p-around--medium{
    padding:0 !important;
}
.slds-modal__content{
    overflow-y:hidden !important;
    height:unset !important;
    max-height:unset !important;
}
```
![](../img/20171212181336.png)

Now we get a modal which is similar to standard action.
![](../img/20171212181350.png)
## Reusable component
I created a template modal component which can be used in a component implemented force:lightningQuickActionWithoutHeader. It is available on [this repository](https://github.com/shunkosa/lightningaction-modal).

## Usage
```
<aura:component implements="force:lightningQuickActionWithoutHeader">
	<c:LightningActionModal title="Hello world!" percentWidth="85" bodyHeight="300">
        //awesome modal body
        <aura:set attribute="footer">
            <lightning:button label="Cancel" variant="neutral"/>
            <lightning:button label="Save" variant="brand" />
        </aura:set>
    </c:LightningActionModal>
</aura:component>
```
## Appendix
[Add button to the footer of a Lightning Quick Action component - Salesforce Stack Exchange](https://salesforce.stackexchange.com/questions/160535/add-button-to-the-footer-of-a-lightning-quick-action-component)
