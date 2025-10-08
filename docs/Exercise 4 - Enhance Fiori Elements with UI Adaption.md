<div class="draftWatermark"></div>

# 练习4 - 使用自定义操作和片段增强Fiori Elements

#### 1. 导航到页面映射并更改fiori为灵活布局
![](vx_images/image-38.png)

#### 2. 打开故事板并为实体**Conversations**添加字段
![](vx_images/image.png)

点击**Show Details**并打开**Properties**标签页

![](vx_images/image-1.png)

添加新属性：
**Name**: image
**Type**: LargeBinary
![](vx_images/image-2.png)

添加注解如下：
**Annotation目标:** Core.MediaType
**Annotation值:** application/pdf
![](vx_images/image-3.png)


#### 2. 为实体Conversations添加对象页面

返回故事板并打开PageMap
![](vx_images/image-4.png)

添加新的对象页面

![](vx_images/image-5.png)

选择**Navigation**为 **conversations (Conversations)**
点击**Add**

![](vx_images/image-6.png)

编辑新的页面

![](vx_images/image-7.png)
![](vx_images/image-8.png)
![](vx_images/image-9.png)
![](vx_images/image-10.png)
![](vx_images/image-11.png)

## 3. 测试上传PDF文档到Conversations实体。
![](vx_images/image-13.png)
![](vx_images/image-14.png)
![](vx_images/image-12.png)
![](vx_images/image-15.png)
![](vx_images/image-16.png)
![](vx_images/image-17.png)
![](vx_images/image-18.png)
![](vx_images/image-19.png)

## 4. 在Conversations实体的对象页面中添加自定义控制器。

![](vx_images/image-20.png)
Controller名称: ConversationsController
![](vx_images/image-21.png)

![](vx_images/image-22.png)

调整控制器代码如下：

```
sap.ui.define(['sap/ui/core/mvc/ControllerExtension','sap/base/security/URLWhitelist' ,'sap/ui/model/json/JSONModel'], function (ControllerExtension,URLWhitelist,JSONModel) {
	'use strict';

	return ControllerExtension.extend('incidentmanagement004.Incidents.ext.controller.ConversationsController', {
		// 这部分允许扩展Fiori元素提供的生命周期挂钩或挂钩
		
		showPDF:async function(oEvent){
			var sImageUrl = oEvent.getModel().getBindings().filter( bn=>{ return bn.sPath=='image' }).at(0).vValue;
			let oPdfmodel = new JSONModel({
				Source: sImageUrl,
				Title: 'pdf',
				Height: "1000px"
			});
			URLWhitelist.add("blob");
			this.base.getExtensionAPI()._view.setModel(oPdfmodel,"pdf");
			this.base.getExtensionAPI()._view.getModel('pdfview').setData({"Viewshow":true});

			alert('hello hero');
		},
		
		
		override: {
			/**
             * 当控制器实例化且其视图控件（如有）已创建时调用。
             * 可以在此处修改视图，绑定事件处理程序并进行其他一次性初始化。
             * @memberOf incidentmanagement004.Incidents.ext.controller.ConversationsController
             */



			onInit: function () {
				// 可以通过this.base.getExtensionAPI访问Fiori元素扩展API
			}
		}
	});
});

```
## 5. 在Conversations实体的对象页面中添加自定义操作和片段。
![](vx_images/image-23.png)
![](vx_images/image-24.png)
![](vx_images/image-27.png)
操作ID: DisplayPDF
按钮文本: 显示PDF文档
处理程序文件: ConversationsController.controller (incidentmanagement004.Incidents.ext.controller.ConversationsController.controller, JS)
处理程序按钮: showPDF

![](vx_images/image-25.png)

标题: PdfViewer
片段名: PdfViewerFrag
![](vx_images/image-26.png)

![](vx_images/image-28.png)

```

<core:FragmentDefinition xmlns:core="sap.ui.core" xmlns="sap.m" xmlns:macros="sap.fe.macros">
    <ScrollContainer id="_IDGenScrollContainer1"
        height="100%"
        width="100%"
        horizontal="true"
        vertical="true" visible="{pdfview>/Viewshow}">
        <FlexBox id="_IDGenFlexBox1" direction="Column" renderType="Div" class="sapUiSmallMargin">
            <PDFViewer id="_IDGenPDFViewer1" source="{pdf>/Source}" isTrustedSource="true" displayType ="Embedded" title="{pdf>/Title}" height="{pdf>/Height}">
                <layoutData>
                    <FlexItemData id="_IDGenFlexItemData1" growFactor="1" />
                </layoutData>
            </PDFViewer>
        </FlexBox>
    </ScrollContainer>
</core:FragmentDefinition>

```

## 6. 测试在自定义片段中查看PDF文档
![](vx_images/image-30.png)
![](vx_images/image-29.png)
![](vx_images/image-31.png)
![](vx_images/image-33.png)
![](vx_images/image-34.png)
![](vx_images/image-35.png)
![](vx_images/image-36.png)
![](vx_images/image-37.png)