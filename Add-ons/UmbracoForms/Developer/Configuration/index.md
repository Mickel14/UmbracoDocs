---
versionFrom: 9.0.0
versionTo: 10.0.0
meta.Title: "Umbraco Forms configuration"
meta.Description: "In Umbraco Forms it's possible to customize the functionality with various configuration values."
---

# Configuration

With Umbraco Forms it's possible to customize the functionality with various configuration values.

## Editing configuration values

All configuration for Umbraco Forms is held in the `appSettings.json` file found at the root of your Umbraco website.  If the configuration has been customized to use another source, then the same keys and values discussed in this article can be applied there.

The convention for Umbraco configuration is to have package based options stored as a child structure below the `Umbraco` element, and as a sibling of `CMS`.  Forms configuration follows this pattern, i.e.:

```json
{
  ...
  "Umbraco": {
    "CMS": {
        ...
    },
    "Forms": {
        ...
    }
  }
}
```

All configuration for Forms is optional. In other words, all values have defaults that will be applied if no configuration is available for a particular key.

For illustration purposes, the following structure represents the full set of options for configuration of Forms, along with the default values. This will help when you need to provide a different setting to understand where it should be applied.

```json
  "Forms": {
    "FormDesign": {
      "DisableAutomaticAdditionOfDataConsentField": false,
      "DisableDefaultWorkflow": false,
      "MaxNumberOfColumnsInFormGroup": 12,
      "DefaultTheme": "default",
      "DefaultEmailTemplate": "Forms/Emails/Example-Template.cshtml",
      "Defaults": {
        "ManualApproval": false,
        "DisableStylesheet": false,
        "MarkFieldsIndicator": "NoIndicator",
        "Indicator": "*",
        "RequiredErrorMessage": "Please provide a value for {0}",
        "InvalidErrorMessage": "Please provide a valid value for {0}",
        "ShowValidationSummary": false,
        "HideFieldValidationLabels": false,
        "MessageOnSubmit": "Thank you",
        "StoreRecordsLocally": true,
        "AutocompleteAttribute": ""
      }
    },
    "Options": {
      "IgnoreWorkFlowsOnEdit": "True",
      "ExecuteWorkflowAsync": "False",
      "AllowEditableFormSubmisisons": false,     // Note the typo here (see below).
      "AppendQueryStringOnRedirectAfterFormSubmission": false,
      "CultureToUseWhenParsingDatesForBackOffice": "",
      "TriggerConditionsCheckOn": "change"
    },
    "Security": {
      "DisallowedFileUploadExtensions": "config,exe,dll,asp,aspx",
      "EnableAntiForgeryToken": true,
      "SavePlainTextPasswords": false,
      "DisableFileUploadAccessProtection": false,
      "DefaultAccessToNewForms": "Grant",
      "ManageSecurityWithUserGroups": false,
      "GrantAccessToNewFormsForUserGroups": "admin,editor"
    },
    "FieldTypes": {
      "DatePicker": {
        "DatePickerYearRange": 10
      },
      "Recaptcha2": {
        "PublicKey": "",
        "PrivateKey": ""
      },
      "Recaptcha3": {
        "SiteKey": "",
        "PrivateKey": ""
      },
      "RichText": {
        "DataTypeId": "ca90c950-0aff-4e72-b976-a30b1ac57dad"
      }
    }
  }
```

## Form design configuration

### DisableAutomaticAdditionOfDataConsentField

This configuration value expects a `true` or `false` value and can be used to disable the feature where all new forms are provided with a default "Consent for storing submitted data" field on creation. Defaults to `false`.

### DisableDefaultWorkflow

This configuration value expects a `true` or `false` value and can be used to toggle if new forms that are created adds an email workflow to send the result of the form to the current user who created the form. Defaults to `false`.

### MaxNumberOfColumnsInFormGroup

This setting controls the maximum number of columns that can be created by editors when they configure groups within a form. The default value used if the setting value is not provided is 12.

### DefaultTheme
This setting allows you to configure the name of the theme to use when an editor has not specifically selected one for a form.  If empty or missing, the default value of "default" is used.  If a custom default theme is configured, it will be used for rendering forms where the requested file exists, and where not, will fall back to the out of the box default theme.

### DefaultEmailTemplate
When creating an empty form, a single workflow is added that will send an email to the current user's address. By default, the template shipped with Umbraco Forms is available at `Forms/Emails/Example-Template.cshtml` is used.

If you have created a custom template and would like to use that as the default instead, you can set the path here using this configuration setting.

### Form default settings configuration

The following configured values are applied to all forms as they are created. They can then be amended on a per-form basis via the Umbraco backoffice.

Once the form has been created, the values are set explicitly on the form, so subsequent changes to the defaults in configuration won't change the settings used on existing forms.

#### ManualApproval

This setting needs to be a `true` or `false` value and will allow you to toggle if a form allows submissions to be post moderated. Most use cases are for publicly shown entries such as blog post comments or submissions for a social campaign. Defaults to `false`.

#### DisableStylesheet

This setting needs to be a `true` or `false` value and will allow you to toggle if the form will include some default styling with the Umbraco Forms CSS stylesheet. Defaults to `false`.

#### MarkFieldsIndicator

This setting can have the following values to allow you to toggle the mode of marking mandatory or optional fields:

* `NoIndicator` (default)
* `MarkMandatoryFields`
* `MarkOptionalFields`

#### Indicator

This setting is used to mark the mandatory or optional fields based on the setting above. By default this is an asterisk `*`.

#### RequiredErrorMessage

This allows you to configure the required error validation message. By default this is `Please provide a value for {0}` where the `{0}` is used to replace the name of the field that is required.

#### InvalidErrorMessage

This allows you to configure the invalid error validation message. By default this is `Please provide a valid value for {0}` where the `{0}` is used to replace the name of the field that is invalid.

#### ShowValidationSummary

This setting needs to be a `true` or `false` value and will allow you to toggle if the form will display all form validation error messages in a validation summary together.  Defaults to `false`.

#### HideFieldValidationLabels

This setting needs to be a `true` or `false` value and will allow you to toggle if the form will show inline validation error messages next to the form field that is invalid.  Defaults to `false`.

#### MessageOnSubmit

This allows you to configure what text is displayed when a form is submitted and is not being redirected to a different content node. Defaults to `Thank you`.

#### StoreRecordsLocally

This setting needs to be a `True` or `False` value and will allow you to toggle if form submission data should be stored in the Umbraco Forms database tables. By default this is set to `True`.

#### AutocompleteAttribute

This setting provides a value to be used for the `autocomplete` attribute for newly created forms.  By default the value is empty, but can be set to `on` or `off` to have that value applied as the attribute value used when rendering the form.

## Package options configuration

### IgnoreWorkFlowsOnEdit

This configuration expects a `True` or `False` string value, or a comma-separated list of form names, and allows you to toggle if a form submission is edited again, that the workflows on the form will re-fire after an update to the form submission. This is used in conjunction with the `AllowEditableFormSubmissions` configuration value. Defaults to `True`.

### ExecuteWorkflowAsync

This configuration key is *experimental* and will allow Workflows to be executed in an asynchronous manner. The value can be a `True` or `False` string value, or a comma-separated list of form names. Defaults to `False`.

### AllowEditableFormSubmissions

This configuration value expects a `true` or `false` value and can be used to toggle the functionality to allow a form submission to be editable and re-submitted. When the value is set to `true` it allows Form Submissions to be edited using the following querystring for the page containing the form on the site. `?recordId=GUID` Replace `GUID` with the GUID of the form submission. Defaults to `false`.

_Note:_ There is a typo in this setting where it has been named as `AllowEditableFormSubmisisons`. This is the name that needs to be used in configuration for Forms 9.  In Forms 10 this will be corrected to `AllowEditableFormSubmissions`.

:::warning
Enable this feature ONLY if you understand the security implications.
:::

### AppendQueryStringOnRedirectAfterFormSubmission

When redirecting following a form submission, a `TempData` value is set that is used to ensure the submission message is displayed rather than the form itself. In certain situations, such as hosting pages with forms in IFRAMEs from other websites, this value is not persisted between requests.

By settting the following value to True, a querystring value of `formSubmitted=<id of submitted form>`, will be used to indicate a form submitted on the previous request.

### CultureToUseWhenParsingDatesForBackOffice
This setting has been added in 9.5 and 10.1, to help resolve an issue with multi-lingual setups.

When Umbraco Forms stores data for a record, it saves the values submitted for each field into a dedicated table for each type (string, date etc.). It also saves a second copy of the record in a JSON structure which is more suitable for fast look-up and display in the backoffice. Date values are serialized using the culture used by the front-end website when the form entry is stored.

When displaying the data in the backoffice, the date value needs to be parsed back into an actual date object for formatting. And this can cause a problem if the backoffice user is using a different language, and hence culture setting, than that used when the value was stored.

From 9.5 and 10.1 onwards, the culture used when storing the form entry is recorded, thus we can ensure the correct value is used when parsing the date. However, this doesn't help for historically stored records. To at least partially mitigate the problem, when you have editors using different languages to a single language presented on the website front-end, you can set this value to match the culture code used on the website. This ensures the date fields in the backoffice are correctly presented.

Taking an example of a website globalization culture code setting of "en-US" (and a date format of `m/d/y`), but an editor uses "en-GB" (which formats dates as of `d/m/y`). By setting the value of this configuration key to "en-US", you can ensure that the culture when parsing dates for presentation in the backoffice will match that used when the value was stored.

If no value is set, and no culture value was stored alongside the form entry, the culture based on the language associated with the current backoffice user will be used.

### TriggerConditionsCheckOn

This configuration setting provides control over the client-side event used to trigger conditions. The `change` event is the default used if this setting is empty. It can also be set to a value of `input`. The main difference seen here relates to text fields, with the "input" event firing on each key press, and the "change" only when the field loses focus.

## Security configuration

### DisallowedFileUploadExtensions

When using the File Upload field in a form, editors can choose which file extensions they want to accept. When an image is expected, they can for example specify that only `.jpg` or `.png` files are uploaded.

There are certain file extensions that in almost all cases should never be allowed, which are held in this configuration value. This means that even if an editor has selected to allow all files, any files that match the extensions listed here will be blocked.

By default, .NET related code files like `.config` and `.aspx` are included in this deny list. You can add or - if you are sure - remove values from this list to meet your needs.

### EnableAntiForgeryToken

This setting needs to be a `true` or `false` value and will enable the ASP.NET Anti Forgery Token and we recommend that you enable this option. Defaults to `true`.

In certain circumstances, including hosting pages with forms in IFRAMEs from other websites, this may need to be set to `false`.

### SavePlainTextPasswords

This setting needs to be a `true` or `false` value and controls whether password fields provided in forms will be saved to the database. Defaults to `false`.

### DisableFileUploadAccessProtection
In Umbraco Forms 9.2.0, protection was added to uploaded files to prevent users from accessing them if they aren't logged into the backoffice and have permission to manage the form for which the file was submitted. As a policy of being "secure by default", the out of the box behavior is that this access protection is in place.

If for any reason you need to revert to the previous behavior, or have other reasons where you want to permit unauthenticated users from accessing the files, you can turn off this protection by setting this configuration value to `true`.

### DefaultAccessToNewForms
In Umbraco Forms 9.3.0, this setting was added to add control over access to new forms.  The default behavior is for all users to be granted access to newly created forms. To amend that to deny access,
the setting can be updated to a value of `Deny`.  A value of `Grant` or configuration with the setting absent preserves the default behavior.

### ManageSecurityWithUserGroups
Umbraco Forms 9.3.0 introduced the ability to administer access to Umbraco Forms using Umbraco's user groups. This can be used instead or in addition to the legacy administration which is at the level of the individual user.  Set this option to `true` to enable the user group permission management functionality.

### GrantAccessToNewFormsForUserGroups
Also introduced in Umbraco Forms 9.3.0, this setting takes a comma-separated list of user group aliases which will be granted access automatically to newly created forms.  This setting only takes effect when `ManageSecurityWithUserGroups` is set to `true`.

There are two "special" values that can be applied within or instead of the comma-separated list.

A value of `all` will give access to the form to all user groups.

A value of `form-creator` will give access to all the user groups that the user who created the form is part of.

## Field type specific configuration

### Date picker field type configuration

#### DatePickerYearRange

This setting is used to configure the Date Picker form field range of years that is available in the date picker. By default this is a small range of 10 years.

### reCAPTCHA v2 field type configuration

#### PublicKey & PrivateKey

Both of these configuration values are needed in order to use the "*Recaptcha2*" field type implementing legacy ReCaptcha V2 from Google. You can obtain both of these values after signing up to create a ReCaptcha key here - <https://www.google.com/recaptcha/admin>

Google has renamed these recently and the `Site Key` refers to `RecaptchaPublicKey` and `Secret Key` is to be used for `RecaptchaPrivateKey`

### reCAPTCHA v3 field type configuration

#### SiteKey & PrivateKey

Both of these configuration values are needed in order to use the "*reCAPTCHA V3 with Score*" field type implementing ReCaptcha V3 from Google.

You can obtain both of these values after signing up to create a ReCaptcha key here:  <https://www.google.com/recaptcha/admin>.

### Rich text field type configuration

#### DataTypeId

Sets the data type Guid to use to obtain the configuration for the rich text field type. If the setting is absent, the value of the default rich text data type created by Umbraco on a new install is used.

---

Prev: [Extending](../Extending/index.md) &emsp; &emsp; &emsp; &emsp; &emsp; &emsp; &emsp; &emsp; Next: [Security](../Security/index.md)
