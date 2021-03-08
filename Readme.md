# ✨ TYPO3 Extension amazing ✨

This extension improves your daily life as TYPO3 editor and integrator by providing unique features and best practices.

- [How to get it?](#how-to-get-it)
  * [Why paid?](#why-paid)
  * [Installation](#installation)
  * [Configuration](#configuration)
- [Features](#features)
  * [Simplify the backend](#simplify-the-backend)
    + [Remove not needed fields from TCA tables](#remove-not-needed-fields-from-tca-tables)
    + [Improve New Record View](#improve-new-record-view)
    + [Override Table Title](#override-table-title)
  * [Improve usergroup permission setup](#improve-usergroup-permission-setup)
    + [Rendering Tables select / modify](#rendering-tables-select--modify)
    + [Show only relevant exclude fields](#show-only-relevant-exclude-fields)
    + [Remove exclude configuration from crucial fields](#remove-exclude-configuration-from-crucial-fields)
  * [Page module](#page-module)
    + [Improved rendering of text](#improved-rendering-of-text)
    + [Styleable backend layouts](#styleable-backend-layouts)
  * [Misc](#misc)
    + [Value Picker for Site language locale](#value-picker-for-site-language-locale)
    + [Improved system news](#improved-system-news)
    + [Expand and Collapse all tables](#expand-and-collapse-all-tables)


## How to get it?

3 possible options are possible:

| Individual | Agency   | GitHub Sponsor                                                |
|:-----------|:---------|:--------------------------------------------------------------|
| 75€ (*)    | 150€ (*) | [at least 20€ tier](https://github.com/sponsors/georgringer/) |

**★ (*) One-time price, valid for _unlimited_ projects for one stable version! ★**

*For access or questions, please contact me via [:email: mail@ringer.it](mailto:mail@ringer.it), [twitter](https://twitter.com/georg_ringer) or slack*

### Why paid?

Sponsorships allow me to spend more time for TYPO3, no matter if it is about core development, maintaining extensions or giving support on stackoverflow, GitHub or Slack.

### Installation

**- t3x**

Use the provided t3x file and upload it in the extension manager.

**- composer**

With the provided token you can simply do:

```bash
composer config --auth http-basic.georgringer.repo.packagist.com token <your-personal-token>
composer config repositories.private-packagist composer https://georgringer.repo.packagist.com/<your-personal-username>/
composer require georgringer/amazing
```

### Configuration

After finishing the installation, head over to the extension settings and enable all features you want.

## Features

Check out the list of features:

### Simplify the backend

#### Remove not needed fields from TCA tables
The backend contains tons of fields and most of them are important but if a field is really not needed, it is now hidden

- If a site does only use *one* language, **all** language fields of **all** tables are hidden.
- If no category record has been created, the category fields created via the `CategoryRegistry` are hidden.
- If no fe_group record has been created, there is no need for fe_group relations.

#### Improve New Record View

The view *New Record* renders a list of all records which can be created via list module.
The list size has been reduced by:

- Always hide the following records:
  - `backend_layout`: A backend layout should be creating by PageTsConfig
  - `sys_template`: If you still need sys_template records, just use the Template module
  - `sys_news`: See below
- The selection of records `fe_users` and `fe_groups` is only shown if the field `Contains Plugin` on the page is set to *Website Users*.

#### Override Table Title

If records of an extension (or the core) are used for different purposes it helps editors to label those records properly.

The following screenshot shows a record of EXT:news. By default the title would be `News` but it has been overridden with `Blog`.
Other examples could be to call records of `tt_address` **Contact** or **Sales Person**.

![Override Table Title](Resources/Public/Screenshots/recordlist-title.png)

*Configuration*: By using the following PageTsConfig the title can be changed for any page:

```typo3_typoscript
mod.web_list.alternativeTableTitles {
    tx_news_domain_model_news = Blog
    tt_address = EXT:site_package/Resources/Private/Language/locallang.xlf:ttAddress.title
}
```


### Improve usergroup permission setup
Setting up permissions for usergroups is always a big hassle.
This extension will help you save time and reduce mistakes:

#### Rendering Tables select / modify

Permissions on tables are splitted up on read (=select) and write (=modify).
The rendering is improved by rendering both fields next to each other.

![Simplify select and modify](Resources/Public/Screenshots/begroups_select-modify.png)

#### Show only relevant exclude fields

The field **Allowed excludefields** contains a list of *all* tables with all fields.
In most projects a specific usergroup is created for a specific role, e.g. maintaining events or news records.

Given the example from above, only the list of fields of those 2 tables are shown in this usergroup.

![Exclude fields](Resources/Public/Screenshots/begroups_exclude.png)


#### Remove exclude configuration from crucial fields

Most fields within the core are defined as exclude field and need to be enabled for every editor.
However, some of those are kind of mandatory to have a working backend - therefore the exclude setting is removed and those fields are available for editors automatically:

```
- pages
  - slug
  - hidden
  - doktype
- tt_content
  - hidden
  - colPos
  - sys_language_uid
```

### Page module

#### Improved rendering of text

By default, text in the pagemodule is being parsed through PHPs `strip_tags()`. The downside is that this removes all the markup and it makes it hard to read the text.

![Pagemodule](Resources/Public/Screenshots/pagemodule.png)

#### Styleable backend layouts

> TYPO3 10 LTS and setting *new fluid-based page module* are required

The backend layout reflects the actual structure of the rendered page in the frontend.
The ability of using css to style the background of each column helps editors to identify
those columns which is especially useful if a lot of columns are used.

![Backend Layout styles](Resources/Public/Screenshots/backendlayout-styles.png)

Configuration is done within the TsConfig:

```typo3_typoscript
columns {
    1 {
        name = Lightbox Content
        styles = background: repeating-linear-gradient(45deg,#606DBC,#606DBC 10px,#465298 10px,#465298 20px);
        colPos = 4
        colspan = 12
    }
}
```

### Misc

#### Value Picker for Site language locale

Having a value picker makes it easier and less error prone to setup the correct locale of a site language.
In opposite to a dropdown, a value picker can be used to suggest items and it is still possible to use completly different values.

![Value Picker](Resources/Public/Screenshots/locale.png)


#### Improved system news
*10 LTS only*

> System news which are shown below the Backend Login screen and can provide helpful information to other editors.

The news are regular records which need to be created within the backend.
To make those news really usable, they can now be maintained via `AdditionalConfiguration.php`:

```php
$GLOBALS['TYPO3_CONF_VARS']['EXTENSIONS']['amazing']['system_news'] = [
    [
        'header' => 'I am a system news',
        'content' => 'This feels<br><strong>awesome</strong>!',
        'date' => '1.10.2020'
    ]
];
```

![Login](Resources/Public/Screenshots/login.png)


Advantages are:
- Deploy and maintain changes just as any code
- Different news depending on the environment

#### Expand and Collapse all tables

A new button in the toolbar of the list module allows expanding and collapsing all tables with one click.
