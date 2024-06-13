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
![](https://github.com/silviosotelo/apex-search-bar/blob/main/preview.png)


## Install
- Import plugin file "dynamic_action_plugin_com_rws_searchbar.sql" from **dist** directory into your application
- *Optional:* Deploy the JS/CSS files from **src/files** directory on your web server and change the "Plugin File Prefix" to web servers folder path.
- *Optional:* Compile the plugin PL/SQL package in your APEX parsing schema and change the plugin render/ajax function to include the package object name. The package files are located in **src/db** directory.


## Plugin Settings
The plugin settings are highly customizable and you can change:

### Application settings
- **Search Placeholder Text** - Text that is displayed in the spotlight search input field as an placeholder
- **Minimum Characters Text** - Text that is displayed when not enough characters are entered to activate search feature
- **No Match Found Text** - Text that is displayed when no search result was found

### Component settings
- **Data Source SQL** - SQL Query which returns the data which can be searched through search
  - **Column 1:** - Code / SKU
  - **Column 2:** - Name / Description
  - **Column 3:** - Price
  - **Column 4:** - Sale Price
  - **Column 5:** - Link / URL
  - **Column 6:** - Image / Icon
- **Page Items to Submit** - Enter page or application items to be set into session state when the SQL query is executed via an AJAX request
- **Show Processing** - Specify whether a waiting / processing indicator is displayed or not. This will replace the default search icon with an spinner as long as data is fetched from database
- **Max. Search Display Results** - The maximum allowed search results displayed at once
- **Search Placeholder Text** - Text that is displayed in the spotlight search input field as an placeholder, Overrides application wide plugin setting
- **Search Placeholder Icon** - Icon that is displayed in the spotlight search input field as an placeholder

## How to use
- Add New Region in Page 0
- Add New Page Item in the same region
- New DA on *Page Load* event
- New Action: *APEX Search Bar*
- Choose Affected Item to Previus Page Item
- Choose Affected Elements to Previus Region
- Choose best config's


#### Sample SQL Query for data source

```language-sql
select 'Oracle Application Express JavaScript API Reference' as name
      ,'ORCL-APEX-JSAPI' as code
      ,null as price
      ,null as sale_price
      ,'https://docs.oracle.com/en/database/oracle/application-express/19.2/aexjs/index.html' as url
      ,null as image
from   dual
union all
select 'Oracle APEX Cards Regions' as name
      ,'ORCL-APEX-CARDS-REGIONS' as code
      ,null as price
      ,null as sale_price
      ,'https://apex.oracle.com/pls/apex/r/apex_pm/ut/card-regions' as url
      ,null as image
from   dual
union all
select 'Oracle APEX Interactive Grid' as name
      ,'ORCL-APEX-IG' as code
      ,null as price
      ,null as sale_price
      ,'https://blog.cloudnueva.com/apex-ig-processing-selected-records' as url
      ,null as image
from   dual
```


## Demo Application
https://apex.oracle.com/pls/apex/f?p=RWS


## Changelog

#### 1.0.5 - Initial Release


## License
MIT
