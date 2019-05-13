---
title: Using adaptive cards in your skill
description: Learn how to use adaptive cards in your bot-based skill.
label: Conceptual

ms.assetid: A7CD987E-5DD1-42EA-A436-49D4E8327365
ms.date: 05/10/2019
ms.topic: article

keywords: cortana
---

# Use adaptive cards in Your Cortana Skill

Cards are interface elements that you can use to enhance the user experience in your Cortana skill. As with all cards, you can only use them when Cortana is running on a device with a display. See [Determine Cortana's device type](./cortana-device-type.md) to get device information.
  
An adaptive card is the most versatile display card. It's customizable, and can include any combination of text, speech, images, buttons, and input fields.  

## Adaptive cards  

Adaptive cards provide the following options.  

|     |     |
| --- | --- |
|**Input controls** | Add input controls for text, date, number, time, toggle switch, and choice set.  |
|**Richer text** | Text in your card is not limited to title, subtitle, and text fixed formats. Use a variety of font sizes, formats, and colors. |
|**A single open card exchange format** | Use your existing cards in a common and consistent way and extend your cards with rich controls using a common schema.  |

Adaptive cards use the open card exchange format. This format enables you to specify user interface content for all cards in your skill in a common and consistent way. You describe the content as a simple JSON object. The JSON content is natively displayed by the skill and automatically adapts to the look and feel of your skill.

>[!Important]
>Cortana currently supports Adaptive Cards version 1.0. only.

Adaptive cards include elements, containers, actions, and inputs. A basic adaptive card includes:

* an adaptive card root object,
* an adaptive card body, which includes the elements of your card, and
* actions for your adaptive card, which are typically displayed in an action bar at the bottom of your card.  

## Adaptive Cards Designer and Visualizer

The Adaptive Cards Designer provides an interactive card builder where you can see the resulting card JSON data.

* [adaptivecards.io/designer](https://adaptivecards.io/designer)

The Adaptive Cards Visualizer shows you what the JSON data will look like onscreen.

* [adaptivecards.io/visualizer](https://adaptivecards.io/visualizer)

>[!IMPORTANT]

> 1. In order to have Cortana speak the text from an adaptive card, the speak object from the adaptive card needs to be copied to the `Message for Cortana`.
> 1. Make sure the speak object text is wrapped in SSML `<speak>` tags. (See the Cortana [Speech Synthesis Markup Language (SSML) reference](https://docs.microsoft.com/en-us/cortana/skills/speech-synthesis-markup-language) for details.)

 The sample text block provides the title text for the sample card in the Adaptive Cards Visualizer.

 ```json
 "type": "TextBlock",
 "text": "Publish adaptive card schema",
 "weight": "bolder",
 "size": "medium"
 ```

 ![Sample card](../media/images/ac_visualizer1.png)  

 You can change the title text by updating the text block.

 ```json
 "type": "TextBlock",
 "text": "This is a test",
 "weight": "bolder",
 "size": "medium"
 ```  

 ![Revised card](../media/images/ac_visualizer2.png)  

## Create an adaptive card using .NET

1. Install the `AdaptiveCards` NuGet package.
1. Specify the elements of your card in code.
1. Add the card to your Cortana skill as an attachment.

The following code adds an adaptive card to a Cortana skill response for Bot Framework V3.

```csharp
 var response = context.MakeMessage();

 AdaptiveCard card = new AdaptiveCard();

 card.Body.Add(new AdaptiveTextBlock()
     {
         Text = "This is a test",
         Weight = TextWeight.Bolder
         Size = TextSize.Medium,
     }
 );

 response.Attachments.Add(card.ToAttachment());

 await context.PostAsync(response);
 context.Wait(MessageReceived);
```
 
```javascript
let msg = new builder.Message(session)
// in V3, there is no AdaptiveCard builder or helper, so we send the JSON
msg.addAttachment(
{
    "contentType": "application/vnd.microsoft.card.adaptive",
    "content": {
        "$schema": "http://adaptivecards.io/schemas/adaptive-card.json",
        "type": "AdaptiveCard",
        "version": "1.0",
        "body": [{
            "type": "TextBlock",
            "text": "This is a test",
            "size": "medium",
            "weight": "bolder"
        }]
    }
});
session.send(msg);
 ```

## More Information  

* For more information about adaptive cards, visit the  [adaptivecards.io](https://adaptivecards.io) page.  
* For more information about Bot Framework cards using .Net, visit the [Add rich card attachments to messages](https://docs.microsoft.com/azure/bot-service/dotnet/bot-builder-dotnet-add-rich-card-attachments?view=azure-bot-service-3.0) for .NET page.
* For more information about Bot Framework cards using Node.js, visit the [Add rich card attachments to messages](https://docs.microsoft.com/azure/bot-service/nodejs/bot-builder-nodejs-send-rich-cards?view=azure-bot-service-3.0) for Node.js page.
* For an example of creating an adaptive card using .NET, visit the [Add an adaptive card to a message](https://docs.microsoft.com/azure/bot-service/dotnet/bot-builder-dotnet-add-rich-card-attachments?view=azure-bot-service-3.0#adaptive-card) section of the [Add rich card attachments to messages](https://docs.microsoft.com/en-us/azure/bot-service/dotnet/bot-builder-dotnet-add-rich-card-attachments?view=azure-bot-service-3.0#adaptive-card) page.  
