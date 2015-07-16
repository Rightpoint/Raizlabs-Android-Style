## [Raizlabs Android Libraries](id:tableOfContents)<a name="tableOfContents"></a>

This document consists of all the raizlabs android open source libraries we use in our Raizlabs projects.


[AndroidWebServiceManager](#AndroidWebServiceManager)

[Broker](#Broker)

[CoreUtils](#CoreUtils)

[DBFlow](#DBFlow)

[UniversalAdapter](#UniversalAdapter)




### <a name="AndroidWebServiceManager"></a>[AndroidWebServiceManager](https://github.com/Raizlabs/AndroidWebServiceManager)

An efficient http client wrapper library that handles all the multi-threaded web requests and responses asynchronously. 

- Some of the features are:
	- Request priority & scheduling.
	- Connection pooling.
	- Support to REST api calls such as GET and POST.
	- Authentication.

[back to top](#tableOfContents)

### <a name="Broker"></a>[Broker](https://github.com/Raizlabs/Broker)

A façade between executing requests and creating them. The library provides a wrapper for creating requests, but delegates the actual execution to RequestExecutors to enable a unified API for requests. It also utilizes annotation processing when creating REST interfaces to simplify code you write dramatically.

[back to top](#tableOfContents)

### <a name="CoreUtils"></a>[CoreUtils](https://github.com/Raizlabs/CoreUtils)

Contents needed.

- Some of the features are:
	- Contents needed.
	
[back to top](#tableOfContents)

### <a name="DBFlow"></a>[DBFlow](https://github.com/Raizlabs/DBFlow)

A robust, powerful, and very simple ORM android database library with annotation processing.

The library is built on speed, performance, and approachability. It not only eliminates most boiler-plate code for dealing with databases, but also provides a powerful and simple API to manage interactions.

- Some of the features are:
	- Many, many unit tests on nearly every feature.
	- Built on maximum performance using annotation processing, lazy-loading, and speed-tests [here](https://github.com/Raizlabs/AndroidDatabaseLibraryComparison).
	- Built-in model caching for blazing-fast retrieval and very flexible customization.
	- Powerful and fluid SQL-wrapping statements that mimic real SQLite queries
	- Triggers, Views, Indexes, and many more SQLite features.
	- Seamless multi-database support.
	- Direct-to-database parsing for data such as JSON
	- Flexibility in the API enabling you to override functionality to suit your needs.
	- ContentProvider generation using annotations
	- Content Observing using Uri

[back to top](#tableOfContents)

### <a name="UniversalAdapter"></a>[UniversalAdapter](https://github.com/Raizlabs/UniversalAdapter)

A single adapter implementation for any scrolling view or ViewGroup.

This library consolidates the differences between BaseAdapter, RecyclerView.Adapter, PagerAdapter, and binding to ViewGroup into one unified API.

Its underlying implementation is based on the ViewHolder pattern and abstracts it to work with all of the adapter views. 

- Some of the features are:
	- One adapter implementation to rule them all. Enforces ViewHolder pattern and binds to a range of ViewGroup such as LinearLayout, GridView, ListView, RecyclerView, ViewPager, and more!
	- List observability: when an adapter's inner content changes, notifications to the parent adapter view happen automatically.
	- Merged adapter: add an arbitrary amount of UniversalAdapter's together to enable diverse view-data sets!
	- Unified header and footer support: add an arbitrary number of headers and footers to any UniversalAdapter and let the library handle it for you, no matter the parent view!

[back to top](#tableOfContents)
