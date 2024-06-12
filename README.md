# Oracle APEX Dynamic Action Plugin - APEX Search Bar

APEX Search Bar is a powerful search feature (like on Amazon) to search. It provides quick navigation and unified search experience across an APEX application.


- [Oracle APEX Dynamic Action Plugin - APEX Search Bar](#oracle-apex-dynamic-action-plugin-apex-search-bar)
	- [Preview](#preview)
	- [Install](#install)
	- [Plugin Settings](#plugin-settings)
		- [Application settings](#application-settings)
		- [Component settings](#component-settings)
	- [Plugin Events](#plugin-events)
	- [How to use](#how-to-use)
			- [Sample SQL Query for data source](#sample-sql-query-for-data-source)
	- [Demo Application](#demo-application)
	- [Changelog](#changelog)
	- [License](#license)


## Preview
![](https://github.com/Dani3lSun/apex-plugin-spotlight/blob/master/preview.gif)


## Install
- Import plugin file "dynamic_action_plugin_com_rws_searchbar.sql" from **dist** directory into your application
- *Optional:* Deploy the JS/CSS files from **src/files** directory on your web server and change the "Plugin File Prefix" to web servers folder path.
- *Optional:* Compile the plugin PL/SQL package in your APEX parsing schema and change the plugin render/ajax function to include the package object name. The package files are located in **src/db** directory.


## Plugin Settings
The plugin settings are highly customizable and you can change:

### Application settings
- **Search Placeholder Text** - Text that is displayed in the spotlight search input field as an placeholder
- **More Characters Text** - Text that is displayed when not enough characters are entered to activate search feature
- **No Match Found Text** - Text that is displayed when no search result was found
- **1 Match Found Text** - Text that is displayed when 1 search result was found
- **Multiple Matches Found Text** - Text that is displayed when multiple search results were found
- **In-Page Search Text** - Text that is displayed in the spotlight search list for in-page search feature
- **Search History Delete Text** - Text that is displayed in the spotlight search history list to delete local search history. Only valid when you enabled search history feature

### Component settings
- **Enable Keyboard Shortcuts** - Enables you to add custom keyboard shortcuts to open spotlight search
- **Keyboard Shortcuts** - You can specify multiple keyboard shortcuts, just separate the different shortcuts by a comma
- **Data Source SQL** - SQL Query which returns the data which can be searched through spotlight search
  - **Column 1:** - Name / Title
  - **Column 2:** - Description
  - **Column 3:** - Link / URL
  - **Column 4:** - Icon
	- **Column 5:** - Icon Background Color (optional)
- **Page Items to Submit** - Enter page or application items to be set into session state when the SQL query is executed via an AJAX request
- **Show Processing** - Specify whether a waiting / processing indicator is displayed or not. This will replace the default search icon with an spinner as long as data is fetched from database
- **Enable Local Data Cache** - Enable data cache to save the complete server response (from Data Source AJAX call) in session storage of browser. This helps to reduce calls from browser to server side
- **Enable In-Page Search** - Enable in-page search to highlight found results on the current page depending on the search keyword
- **Enable Prefill Selected Text** - Enable prefill selected text to automatically set the spotlight search input to your selected / marked text when it opens
- **Max. Search Display Results** - The maximum allowed search results displayed at once
- **Width** - Width of the Spotlight search dialog. Enter either numbers for pixel values or percentage values
- **Theme** - Choose the Spotlight Search theme which best matches your current UI
- **Search Placeholder Text** - Text that is displayed in the spotlight search input field as an placeholder, Overrides application wide plugin setting
- **Search Placeholder Icon** - Icon that is displayed in the spotlight search input field as an placeholder
- **Enable Search History** - Enable search history to show a popover (on mouse hover of main search icon next to spotlight input field) which contains the last 20 search terms of the user. It uses browsers local storage to store the entered search terms
- **Escape Special Characters** - To prevent Cross-Site Scripting (XSS) attacks, always set this attribute to Yes. If you need to render HTML tags stored in the page item or in the entries of a list of values, you can set this flag to No


## Plugin Events
- **On Open** - DA event that fires when the Spotlight Search dialog opens
- **On Close** - DA event that fires when the Spotlight Search dialog closes
- **Executed In-Page Search** - DA event that fires when an in-page search is executed - *this.data* holds the search keyword
- **Get Server Data Error** - DA event that fires when the AJAX functions which returns the data (based on your SQL Query) has an error - *this.data* holds the error message
- **Get Server Data Success** - DA event that fires when the AJAX functions which returns the data (based on your SQL Query) is successful - *this.data* holds the JSON object returned by the server


## How to use
- New DA on *Page Load* event
- New Action: *APEX Search Bar*
- Choose best fitting settings


#### Sample SQL Query for data source

```language-sql
SELECT aap.page_title AS title
      ,'Go to page > ' || aap.page_title AS description
      ,apex_page.get_url(p_page => aap.page_id) AS link
      ,'fa-arrow-right' AS icon
  FROM apex_application_pages aap
 WHERE aap.application_id = :app_id
   AND aap.page_mode = 'Normal'
   AND aap.page_requires_authentication = 'Yes'
   AND aap.page_id != 0
 ORDER BY aap.page_id
```

```language-sql
SELECT aap.page_title AS title
      ,'Go to page > ' || aap.page_title AS description
      ,apex_page.get_url(p_page => aap.page_id) AS link
      ,'fa-arrow-right' AS icon
			,'#479d9d' AS icon_color
  FROM apex_application_pages aap
 WHERE aap.application_id = :app_id
   AND aap.page_mode = 'Normal'
   AND aap.page_requires_authentication = 'Yes'
   AND aap.page_id != 0
 ORDER BY aap.page_id
```

```language-sql
SELECT aale.entry_text AS title
      ,CASE
         WHEN aale.parent_entry_text IS NULL THEN
          'Application > ' || aale.entry_text
         ELSE
          'Application > ' || aale.parent_entry_text || ' > ' ||
          aale.entry_text
       END AS description
      ,apex_util.prepare_url(p_url => apex_plugin_util.replace_substitutions(p_value => aale.entry_target)) AS link
      ,nvl(aale.entry_image
          ,'fa-file-o') AS icon
  FROM apex_application_list_entries aale
 WHERE aale.application_id = :app_id
   AND aale.list_name = 'Desktop Navigation Menu'
```

**Use the search keyword in your link target or URL using substitution string "\~SEARCH_VALUE\~"**

*Note: If an URL contains the substitution string "\~SEARCH_VALUE\~" the resulting list entry is always shown*

```language-sql
SELECT aap.page_title AS title
      ,'Set item with search keyword' AS description
      ,apex_page.get_url(p_page   => aap.page_id
                        ,p_items  => 'P1_ITEM'
                        ,p_values => '~SEARCH_VALUE~') AS link
      ,'fa-home' AS icon
  FROM apex_application_pages aap
 WHERE aap.application_id = :app_id
   AND aap.page_id = 1
```

```language-sql
SELECT 'Set a item' AS title
      ,'Set item with search keyword on client side' AS description
      ,'javascript:$s(''P1_ITEM'',''~SEARCH_VALUE~'');' AS link
      ,'fa-home' AS icon
  FROM dual
```

```language-sql
-- navigation menu list
SELECT aale.entry_text AS title
      ,CASE
         WHEN aale.parent_entry_text IS NULL THEN
          'Application > ' || aale.entry_text
         ELSE
          'Application > ' || aale.parent_entry_text || ' > ' ||
          aale.entry_text
       END AS description
      ,apex_util.prepare_url(p_url => apex_plugin_util.replace_substitutions(p_value => aale.entry_target)) AS link
      ,nvl(aale.entry_image
          ,'fa-file-o') AS icon
  FROM apex_application_list_entries aale
 WHERE aale.application_id = :app_id
   AND aale.list_name = 'Desktop Navigation Menu'
UNION ALL
-- static entry --> app wide search
SELECT 'Search Application' AS title
      ,'Search for customers, products, orders or tags...' AS description
      ,apex_page.get_url(p_page        => 30
                        ,p_clear_cache => 30
                        ,p_items       => 'P30_SEARCH'
                        ,p_values      => '~SEARCH_VALUE~') AS link
      ,'fa-search' AS icon
  FROM dual
```


## Demo Application
https://apex.oracle.com/pls/apex/f?p=APEXPLUGIN


## Changelog

#### 1.0.0 - Initial Release


## License
MIT
