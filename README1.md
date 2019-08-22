# Keyword-Driven using TestCafe (beta)

This is the **Keyword-Driven** framework plugin for [TestCafe](http://devexpress.github.io/testcafe).

[![npm badge](https://miro.medium.com/max/1838/1*VjWsyasduivy_Hkr2LRq2A.png)]()

Testcafe Keyword Driven Framework is a type of Functional Automation Testing Framework which is also known as Table-Driven testing or Action Word based testing.

![report-sample](https://larion.com/wp-content/uploads/2017/04/Keyword-Driven-Framework-Testing.jpg)

## To install this TestCafe Keyword-Driven

- run the command `npm i keyword_driven`.

## Usage

- add to the testcafe command-line the following options:

```sh
testcafe chrome ./path-to-tests/*(.js)
```

## To execute the solution

- install [testcafe](https://devexpress.github.io/testcafe/documentation/getting-started/) (version >= ..):

  - `npm i -g testcafe`
  
  - install [xlsx](https://www.npmjs.com/package/xlsx) (version >= 0.15.1):

  - `npm i xlsx`

- Create a `keyword-driven.js` file at the project root:

```javascript
import { Selector } from 'testcafe';
import XPath from './ComponentHelper/xpath-selector';

fixture `Getting Started`
.page `https://angelswelding.hms2go.no/`;

var XLSX = require('xlsx')
    var workbook = XLSX.readFile('Keyword - Copy.xlsx');
    var sheet_name_list = workbook.SheetNames;
    var xlData = XLSX.utils.sheet_to_json(workbook.Sheets[sheet_name_list[0,1,2,3,4,5,6]]);

    test('Keyword-Driven',  async t => {
        await t.maximizeWindow()
        .pressKey('home right . ctrl + shift + J')
        for (let i = 0; i < xlData.length; i++) {
            let element = xlData[i]
            switch (element.Keyword) {
                case "navigateTo":
                    await t[element.Keyword](element.Parameter)
                    break;
                case "click":
                    await t[element.Keyword](XPath(element.LocatorValue))
                    break;
                case "typeText":
                    await t[element.Keyword](XPath(element.LocatorValue), element.Parameter)
                    break;
                case "selectText":
                    await t[element.Keyword](XPath(element.LocatorValue), element.Parameter)
                    break;
                default:
                    return;
            }
            await t.setTestSpeed(0.1)
        }
    });
    


```

- create the following script in the `xpath-selector.js` file:

```javascript
import { Selector } from 'testcafe';


const elementByXPath = Selector(xpath => {
    const iterator = document.evaluate(xpath, document, null, XPathResult.UNORDERED_NODE_ITERATOR_TYPE, null )
    const items = [];

    let item = iterator.iterateNext();

    while (item) {
        items.push(item);
        item = iterator.iterateNext();
    }

    return items;
});

export default function (xpath) {
    return Selector(elementByXPath(xpath));
}

```

- run the command `testcafe chrome keyword-driven.js`

## Utility

- `testcafe` enables to execute the keyword-driven solution;
- `xlsx` enables to read the excel file with data dynamically from the path:
- `xpath-selector.js` enables to access the xpath
 

## Tags managment

- Tags can be managed through the configuration file `testcafe-reporter-cucumber-json.json`
  - this json file will be created on the first reporter run
- To discard a tag, add this tag to the `noisyTags` section of the json configuration file.

## Error rendering

- this reporter will report multiple code frames, one for each file reported in the stacktrace

```text
1) The specified selector does not match any element in the DOM tree.

   Browser: Chrome 76.0.3809 / Windows 10.0.0
   Screenshot: /Users/HDO/VSCodeProjects/testcafe-starter/screenshots/2018-05-07_10-39-08/test-2/Firefox_59.0.0_Mac_OS_X_10.12.0/errors/1.png

      13 |
      14 |  const value = inputData.name || "";
      15 |
      16 |  await t
      17 |    .setTestSpeed(config.testcafe.testSpeed)
   --------------------------------------------
    → 18 |    .hover(selector.userNameInputBox)
   --------------------------------------------
      19 |    .expect(selector.userNameInputBox.hasAttribute("disabled")).notOk()
      20 |    .typeText(selector.userNameInputBox, value, {replace: true})
      21 |    .pressKey("tab");
      22 |};
      23 |

      at Object.(anonymous) (/Users/HDO/VSCodeProjects/testcafe-starter/domains/testcafe-sample-page/steps/i-enter-my-name.ts:18:6)
      at (anonymous) (/Users/HDO/VSCodeProjects/testcafe-starter/domains/testcafe-sample-page/steps/i-enter-my-name.ts:7:71)
      at __awaiter (/Users/HDO/VSCodeProjects/testcafe-starter/domains/testcafe-sample-page/steps/i-enter-my-name.ts:3:12)
      at exports.default (/Users/HDO/VSCodeProjects/testcafe-starter/domains/testcafe-sample-page/steps/i-enter-my-name.ts:7:36)


       6 |  if (canExecute === false) {
       7 |    return;
       8 |  }
       9 |  const foundStep = stepMappings[stepName];
      10 |  if (typeof foundStep === "function" ) {
   --------------------------------------------
    → 11 |    await foundStep(stepName);
   --------------------------------------------
      12 |    return;
      13 |  }
      14 |  throw new Error(`Step "${stepName}" is not mapped to an executable code.`);
      15 |}
      16 |export async function given(stepName: GivenStep) {

      at (anonymous) (/Users/HDO/VSCodeProjects/testcafe-starter/step-runner.ts:11:11)
      at (anonymous) (/Users/HDO/VSCodeProjects/testcafe-starter/step-runner.ts:7:71)
      at __awaiter (/Users/HDO/VSCodeProjects/testcafe-starter/step-runner.ts:3:12)
      at executeStep (/Users/HDO/VSCodeProjects/testcafe-starter/step-runner.ts:14:12)
      at Object.(anonymous) (/Users/HDO/VSCodeProjects/testcafe-starter/step-runner.ts:20:9)
      at (anonymous) (/Users/HDO/VSCodeProjects/testcafe-starter/step-runner.ts:7:71)
      at __awaiter (/Users/HDO/VSCodeProjects/testcafe-starter/step-runner.ts:3:12)
      at Object.when (/Users/HDO/VSCodeProjects/testcafe-starter/step-runner.ts:34:12)


      19 |  await  then("no name should be populated");
      20 |  await   and("I cannot submit my feedback on testcafe");
      21 |});
      22 |
      23 |test("Scenario: can send feedback with my name only", async () =) {
   --------------------------------------------
    → 24 |  await  when("I enter my name");
   --------------------------------------------
      25 |  await  then("I can submit my feedback on testcafe");
      26 |});
      27 |
      28 |test("Scenario: send feedback", async () =) {
      29 |  await env.only( "devci");

      at Object.(anonymous) (/Users/HDO/VSCodeProjects/testcafe-starter/features/testcafe-sample-page.spec.ts:24:10)
      at (anonymous) (/Users/HDO/VSCodeProjects/testcafe-starter/features/testcafe-sample-page.spec.ts:7:71)
      at __awaiter (/Users/HDO/VSCodeProjects/testcafe-starter/features/testcafe-sample-page.spec.ts:3:12)
      at test (/Users/HDO/VSCodeProjects/testcafe-starter/features/testcafe-sample-page.spec.ts:23:66)

```

## Screenshot rendering

- this reporter embeds all screenshots as base 64 images, making the generated json file completely autonomous.

## Video rendering

- this video embeds all actions as .mp4, making the generated .mp4 file completely autonomous.

## Logger rendering

- this logger embeds all information about the execution as a html report, making the generated html file completely autonomous.

## Mail rendering

- this making the emailable logger file completely autonomous.


## Contributors

- [RemigiusLourdusamy](https://github.com/RemigiusL/)


