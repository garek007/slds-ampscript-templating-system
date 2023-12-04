# SLDS Templating System for Marketing cloud
The goal of this is to make it easier to build complex markup in MC without having to wade through so much code. It has been tricky because if you change one thing it cascades and you can break all of your stuff. I need to create some documentation for this, just not sure this idea is worthwhile...

We declare variables in the SLDS Globals file, so that you don't have to remember Content Block IDs. Ultimately, this may have worked better using ContentBlockByName with a predictable file structure. 

For this to work you'll obviously have to include the SLDS css in the head of your document. 

## Usage
```
%%[
Set @id = 'CampaignID'
Set @label = 'Campaign ID'
Set @required = 'true'
Set @toppad = 'large'
Set @padding = 'large'
Set @classes = 'slds-col slds-medium-size_1-of-2 slds-size_1-of-1'
Output(ContentBlockById(@input))
]%%
```
## This will call the slds-input.ampscript file given this code
```
%%[
/*The accept stuff is in case you want to use a text input as a file upload input*/
If Not Empty(@inputAccept) Then
  Set @accept = Concat('accept="',@accept,'"')
endif
]%%
<div class="slds-form-element %%=IIF(Not Empty(@padding),Concat('slds-p-around_',@padding),'')=%% %%=IIF(Not Empty(@toppad),Concat('slds-p-top_',@toppad),'')=%% %%=IIF(Not Empty(@classes),@classes,'')=%% %%=IIF(Not Empty(@bottompad),Concat('slds-p-bottom_',@bottompad),'')=%%">
  <label class="slds-form-element__label" for="%%=v(@id)=%%">
     
    %%=v(@label)=%%
  
    %%[if @required == 'true' Then]%%
  <abbr class="slds-required" title="required">* </abbr>
    %%[endif]%%    
  
  </label>
  <div class="slds-form-element__control">
  <input type="%%=IIF(Empty(@inputType),'text',@inputType)=%%" %%=v(@accept)=%% id="%%=v(@id)=%%" name="%%=v(@id)=%%" %%=IIF(@required=='true','required','')=%% class="slds-input" %%=IIF(Empty(@placeholder),'',Concat('placeholder="',@placeholder,'"'))=%% data-object="%%=v(@object)=%%" %%=IIF(Not Empty(@maxlength),Concat('maxlength="',@maxlength,'"'),'')=%%/>
  </div>
</div>
```

## And produce the HTML markup

```
<div class="slds-form-element slds-p-around_large slds-p-top_large slds-col slds-medium-size_1-of-2 slds-size_1-of-1">
  <label class="slds-form-element__label" for="CampaignID">
     
    Campaign ID

  <abbr class="slds-required" title="required">* </abbr>
  
  
  </label>
  <div class="slds-form-element__control">
  <input type="text" id="CampaignID" name="CampaignID" required class="slds-input" data-object="" />
  </div>
</div>
```