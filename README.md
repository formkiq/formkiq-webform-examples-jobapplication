
# FormKiQ Webform Examples - Job Application Form
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)

This repository contains an example of how to create a simple web form and use [FormKiQ Core](https://github.com/formkiq/formkiq-core) to process and store the responses.

This example uses the [FormKiQ Client JavaScript SDK](https://www.npmjs.com/package/formkiq-client-sdk-javascript) and [assumes you have deployed FormKiQ Core into your AWS account](https://github.com/formkiq/formkiq-core#installation).

As well, this example assumes that your FormKiQ Core deployment has EnablePublicUrls set to **true**.

# Auto-Wired Forms

The FormKiQ Client JavaScript SDK includes an optional auto-wiring function. In order to have your form automatically submit to your FormKiQ Core deployment, you can add a className of "fkq-form" to your &lt;form&gt; element.

Any input fields that should be required should include a "required" HTML Attribute, and those will prevent the form from submitting. To create custom validation rules, you should submit the form manually using **onsubmit**, and add extra validation in the triggered submit function.

# Submitting a Form Manually

If you would rather handle the form manually, you can use the FormKiQ Client SDK to access the FormKiQ Core API. You will still receive the same onFormSubmitted and onFormCompleted events.

## Example

```html
<form name="Job Form" onsubmit="submitForm(this);return false;">
    [...]
</form>

<script>
    let formkiqClient;

    window.onload = () => {
        formkiqClient = new FormkiqClient('<ID>.execute-api.<REGION>.amazonaws.com');
    }

    function submitForm(thisForm) {

        // TODO: additional/custom validation

        formkiqClient.webFormsHandler.submitFormkiqForm(thisForm);
    }

    function onFormSubmitted(formName) {
        console.log(`${formName} has been submitted for processing.`);
    }

    [...]

</script>
```

# The Form Element Name Attribute

By default, the name of your form will also be submitted as a Tag to FormKiQ Core, to help differentiate if you have multiple web forms set up to submit to FormKiQ.

# File Attachments

File attachments are automatically found and set up by the WebFormHandler that comes with the FormKiQ Client JavaScript SDK. The initial form is submitted to FormKiQ Core to create a Form Document, and then any attachments are submitted as Child Documents. This allows you to access them from their parent form in the FormKiQ Console or through the FormKiQ Core API.

# Customizing the data sent to FormKiQ Core

Rather than using the WebFormHandler that comes with the FormKiQ Client SDK, you can create your own data parameters to send directly to the FormKiQ Public POST /documents Endpoint.

NOTE: only the Public POST /documents Endpoint should be used, as you do not want to expose your FormKiQ Core authentication credentials in client-side code.

# Try It Out

Once you have added your FormKiQ Core HTTP API URL (see the inline comments in index.html for more info), you can test this FormKiQ Web Form out by running http-server:

You can test this FormKiQ Web Form out by running http-server:
```sh
npx http-server
```

![Screenshot of Job Application Form Example](https://raw.githubusercontent.com/formkiq/formkiq-webform-examples-jobapplication/master/screenshot.png)