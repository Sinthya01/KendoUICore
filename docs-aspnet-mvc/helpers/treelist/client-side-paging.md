---
title: Client-Side Paging
page_title: Client-Side Paging | Kendo UI TreeList HtmlHelper
description: "Learn how to enable the client-side paging feature to the Kendo UI TreeList for ASP.NET MVC."
slug: client_side_paging_mvc_treelist_helper
position: 4
---

# Client-Side Paging

The TreeList supports client-side paging for large sets of data.

To enable the paging functionality, configure the [`Pageable`](http://docs.telerik.com/aspnet-mvc/api/Kendo.Mvc.UI.Fluent/TreeListPageableSettingsBuilder) settings.

> **Important**
> * Remember to set a [`PageSize`](https://docs.telerik.com/aspnet-mvc/api/Kendo.Mvc.UI.Fluent/TreeListPageableSettingsBuilder#methods-PageSize(System.Double)). You can define a `PageSize` in the `Pageable` or in the [`DataSource`](https://docs.telerik.com/aspnet-mvc/api/Kendo.Mvc.UI.Fluent/AjaxDataSourceBuilderBase#methods-PageSize(System.Int32)) settings. If an already existing dataSource instance is passed to the TreeList, then the `PageSize` option has to be set in the dataSource settings and not in the `Pageable` settings.
> * By default, [`ServerOperations`] (https://docs.telerik.com/aspnet-mvc/api/Kendo.Mvc.UI.Fluent/AjaxDataSourceBuilderBase#methods-ServerOperation(System.Boolean)) are enabled in the Grid MVC wrapper. For client-side paging you have to disable the server operations by setting the `ServerOperations` to `false`.

###### Example

    @(Html.Kendo().TreeList<Kendo.Mvc.Examples.Models.TreeList.EmployeeDirectoryModel>()
        .Name("treelist")
        .Columns(columns =>
        {
            columns.Add().Field(e => e.FirstName).Title("Name").Template("#: FirstName # #: LastName #");
            columns.Add().Field(e => e.Position);
            columns.Add().Field(e => e.HireDate).Format("{0:MMMM d, yyyy}");
        })
        .Filterable()
        .Sortable()
        .DataSource(dataSource => dataSource
            .ServerOperation(false)
            .Read(read => read.Action("All", "EmployeeDirectory"))
            .Model(m => {
                m.Id(f => f.EmployeeId);
                m.ParentId(f => f.ReportsTo);
                m.Field(f => f.ReportsTo);
                m.Expanded(true);
            })
        )
        .Height(540)
        .Pageable(p => p.PageSize(15)
                        .PageSizes(true)
        )
    )


> * When client-side paging is used with editing and an item is added, if the "id" field of the model should be nullable (e.g. `int?`), then the model should be configured to have a default "id" field value on the client-side that is **different** from the default "parentId" field value. This is required as root TreeList items have their "parentId" field set to the default value for no parent (by default this is equal to `null`, but can be configured from the `dataSource.schema.model.fields[FIELD_NAME].defaultValue` option). If this is the case, the default value of the "id" field (null) will be equal to the default "parentId" field value (null), which creates a circular dependency. Setting the default "id" field to a different value (e.g. zero) can be done instead.

###### Example
    @(Html.Kendo().TreeList<Kendo.Mvc.Examples.Models.TreeList.EmployeeDirectoryModel>()
        .Name("treelist")
        ...
        .DataSource(dataSource => dataSource
            .ServerOperation(false)
            .Read(read => read.Action("All", "EmployeeDirectory"))
            .Model(m => {
                // assuming the "EmployeeId" field is equal to null by default and
                // the default value of the "ReportsTo" field is also null
                m.Field(f => f.EmployeeId).DefaultValue(0);
                m.Id(f => f.EmployeeId);
                m.ParentId(f => f.ReportsTo);
                m.Field(f => f.ReportsTo);
                m.Expanded(true);
            })
        )
        .Height(540)
        .Pageable(p => p.PageSize(15)
                        .PageSizes(true)
        )
    )

## See Also

* [JavaScript API Reference](https://docs.telerik.com/kendo-ui/api/javascript/ui/treelist)
