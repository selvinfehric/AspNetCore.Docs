---
title: ASP.NET Core Blazor `QuickGrid` component
author: guardrex
description: The QuickGrid component is a Razor component for quickly and efficiently displaying data in tabular form.
monikerRange: '>= aspnetcore-8.0'
ms.author: wpickett
ms.custom: mvc
ms.date: 11/12/2024
uid: blazor/components/quickgrid
---
# ASP.NET Core Blazor `QuickGrid` component

[!INCLUDE[](~/includes/not-latest-version-without-not-supported-content.md)]

The [`QuickGrid` component](xref:Microsoft.AspNetCore.Components.QuickGrid) is a Razor component for quickly and efficiently displaying data in tabular form. QuickGrid provides a simple and convenient data grid component for common grid rendering scenarios and serves as a reference architecture and performance baseline for building data grid components. QuickGrid is highly optimized and uses advanced techniques to achieve optimal rendering performance.

## Package

Add a package reference for the [`Microsoft.AspNetCore.Components.QuickGrid`](https://www.nuget.org/packages/Microsoft.AspNetCore.Components.QuickGrid) package.

[!INCLUDE[](~/includes/package-reference.md)]

## Sample app

For various QuickGrid demonstrations, see the [**QuickGrid for Blazor** sample app](https://aspnet.github.io/quickgridsamples/). The demo site is hosted on GitHub Pages. The site loads fast thanks to static prerendering using the community-maintained [`BlazorWasmPrerendering.Build` GitHub project](https://github.com/jsakamoto/BlazorWasmPreRendering.Build).

## QuickGrid implementation

To implement a `QuickGrid` component:

:::moniker range=">= aspnetcore-9.0"

* Specify tags for the `QuickGrid` component in Razor markup (`<QuickGrid>...</QuickGrid>`).
* Name a queryable source of data for the grid. Use ***either*** of the following data sources:
  * <xref:Microsoft.AspNetCore.Components.QuickGrid.QuickGrid%601.Items%2A>: A nullable `IQueryable<TGridItem>`, where `TGridItem` is the type of data represented by each row in the grid.
  * <xref:Microsoft.AspNetCore.Components.QuickGrid.QuickGrid%601.ItemsProvider%2A>: A callback that supplies data for the grid.
* <xref:Microsoft.AspNetCore.Components.QuickGrid.QuickGrid%601.Class%2A>: An optional CSS class name. If provided, the class name is included in the `class` attribute of the rendered table.
* <xref:Microsoft.AspNetCore.Components.QuickGrid.QuickGrid%601.Theme%2A>: A theme name (default value: `default`). This affects which styling rules match the table.
* <xref:Microsoft.AspNetCore.Components.QuickGrid.QuickGrid%601.Virtualize%2A>: If true, the grid is rendered with virtualization. This is normally used in conjunction with scrolling and causes the grid to fetch and render only the data around the current scroll viewport. This can greatly improve the performance when scrolling through large data sets. If you use <xref:Microsoft.AspNetCore.Components.QuickGrid.QuickGrid%601.Virtualize%2A>, you should supply a value for <xref:Microsoft.AspNetCore.Components.QuickGrid.QuickGrid%601.ItemSize%2A> and must ensure that every row renders with a constant height. Generally, it's preferable not to use <xref:Microsoft.AspNetCore.Components.QuickGrid.QuickGrid%601.Virtualize%2A> if the amount of data rendered is small or if you're using pagination.
* <xref:Microsoft.AspNetCore.Components.QuickGrid.QuickGrid%601.ItemSize%2A>: Only applicable when using <xref:Microsoft.AspNetCore.Components.QuickGrid.QuickGrid%601.Virtualize%2A>. <xref:Microsoft.AspNetCore.Components.QuickGrid.QuickGrid%601.ItemSize%2A> defines an expected height in pixels for each row, allowing the virtualization mechanism to fetch the correct number of items to match the display size and to ensure accurate scrolling.
* <xref:Microsoft.AspNetCore.Components.QuickGrid.QuickGrid%601.ItemKey%2A>: Optionally defines a value for `@key` on each rendered row. Typically, this is used to specify a unique identifier, such as a primary key value, for each data item. This allows the grid to preserve the association between row elements and data items based on their unique identifiers, even when the `TGridItem` instances are replaced by new copies (for example, after a new query against the underlying data store). If not set, the `@key` is the `TGridItem` instance.
* <xref:Microsoft.AspNetCore.Components.QuickGrid.QuickGrid%601.OverscanCount%2A>: Defines how many additional items to render before and after the visible region to reduce rendering frequency during scrolling. While higher values can improve scroll smoothness by rendering more items off-screen, a higher value can also result in an increase in initial load times. Finding a balance based on your data set size and user experience requirements is recommended. The default value is 3. Only available when using <xref:Microsoft.AspNetCore.Components.QuickGrid.QuickGrid%601.Virtualize%2A>. 
* <xref:Microsoft.AspNetCore.Components.QuickGrid.QuickGrid%601.Pagination%2A>: Optionally links this `TGridItem` instance with a <xref:Microsoft.AspNetCore.Components.QuickGrid.PaginationState> model, causing the grid to fetch and render only the current page of data. This is normally used in conjunction with a <xref:Microsoft.AspNetCore.Components.QuickGrid.Paginator> component or some other UI logic that displays and updates the supplied <xref:Microsoft.AspNetCore.Components.QuickGrid.PaginationState> instance.
* In the QuickGrid child content (<xref:Microsoft.AspNetCore.Components.RenderFragment>), specify <xref:Microsoft.AspNetCore.Components.QuickGrid.PropertyColumn`2>s, which represent `TGridItem` columns whose cells display values:
  * <xref:Microsoft.AspNetCore.Components.QuickGrid.PropertyColumn%602.Property%2A>: Defines the value to be displayed in this column's cells.
  * <xref:Microsoft.AspNetCore.Components.QuickGrid.PropertyColumn%602.Format%2A>: Optionally specifies a format string for the value. Using <xref:Microsoft.AspNetCore.Components.QuickGrid.PropertyColumn%602.Format%2A> requires the `TProp` type to implement <xref:System.IFormattable>.
  * <xref:Microsoft.AspNetCore.Components.QuickGrid.ColumnBase%601.Sortable%2A>: Indicates whether the data should be sortable by this column. The default value may vary according to the column type. For example, a <xref:Microsoft.AspNetCore.Components.QuickGrid.TemplateColumn%601> is sorted if any <xref:Microsoft.AspNetCore.Components.QuickGrid.TemplateColumn%601.SortBy%2A> parameter is specified.
  * <xref:Microsoft.AspNetCore.Components.QuickGrid.ColumnBase%601.InitialSortDirection%2A>: Indicates the sort direction if <xref:Microsoft.AspNetCore.Components.QuickGrid.ColumnBase%601.IsDefaultSortColumn%2A> is `true`.
  * <xref:Microsoft.AspNetCore.Components.QuickGrid.ColumnBase%601.IsDefaultSortColumn%2A>: Indicates whether this column should be sorted by default.
  * <xref:Microsoft.AspNetCore.Components.QuickGrid.ColumnBase%601.PlaceholderTemplate%2A>: If specified, virtualized grids use this template to render cells whose data hasn't been loaded.
  * <xref:Microsoft.AspNetCore.Components.QuickGrid.ColumnBase%601.HeaderTemplate>: An optional template for this column's header cell. If not specified, the default header template includes the <xref:Microsoft.AspNetCore.Components.QuickGrid.ColumnBase%601.Title>, along with any applicable sort indicators and options buttons.
  * <xref:Microsoft.AspNetCore.Components.QuickGrid.ColumnBase%601.Title>: Title text for the column. The title is rendered automatically if <xref:Microsoft.AspNetCore.Components.QuickGrid.ColumnBase%601.HeaderTemplate> isn't used.

:::moniker-end

:::moniker range="< aspnetcore-9.0"

* Specify tags for the `QuickGrid` component in Razor markup (`<QuickGrid>...</QuickGrid>`).
* Name a queryable source of data for the grid. Use ***either*** of the following data sources:
  * <xref:Microsoft.AspNetCore.Components.QuickGrid.QuickGrid%601.Items%2A>: A nullable `IQueryable<TGridItem>`, where `TGridItem` is the type of data represented by each row in the grid.
  * <xref:Microsoft.AspNetCore.Components.QuickGrid.QuickGrid%601.ItemsProvider%2A>: A callback that supplies data for the grid.
* <xref:Microsoft.AspNetCore.Components.QuickGrid.QuickGrid%601.Class%2A>: An optional CSS class name. If provided, the class name is included in the `class` attribute of the rendered table.
* <xref:Microsoft.AspNetCore.Components.QuickGrid.QuickGrid%601.Theme%2A>: A theme name (default value: `default`). This affects which styling rules match the table.
* <xref:Microsoft.AspNetCore.Components.QuickGrid.QuickGrid%601.Virtualize%2A>: If true, the grid is rendered with virtualization. This is normally used in conjunction with scrolling and causes the grid to fetch and render only the data around the current scroll viewport. This can greatly improve the performance when scrolling through large data sets. If you use <xref:Microsoft.AspNetCore.Components.QuickGrid.QuickGrid%601.Virtualize%2A>, you should supply a value for <xref:Microsoft.AspNetCore.Components.QuickGrid.QuickGrid%601.ItemSize%2A> and must ensure that every row renders with a constant height. Generally, it's preferable not to use <xref:Microsoft.AspNetCore.Components.QuickGrid.QuickGrid%601.Virtualize%2A> if the amount of data rendered is small or if you're using pagination.
* <xref:Microsoft.AspNetCore.Components.QuickGrid.QuickGrid%601.ItemSize%2A>: Only applicable when using <xref:Microsoft.AspNetCore.Components.QuickGrid.QuickGrid%601.Virtualize%2A>. <xref:Microsoft.AspNetCore.Components.QuickGrid.QuickGrid%601.ItemSize%2A> defines an expected height in pixels for each row, allowing the virtualization mechanism to fetch the correct number of items to match the display size and to ensure accurate scrolling.
* <xref:Microsoft.AspNetCore.Components.QuickGrid.QuickGrid%601.ItemKey%2A>: Optionally defines a value for `@key` on each rendered row. Typically, this is used to specify a unique identifier, such as a primary key value, for each data item. This allows the grid to preserve the association between row elements and data items based on their unique identifiers, even when the `TGridItem` instances are replaced by new copies (for example, after a new query against the underlying data store). If not set, the `@key` is the `TGridItem` instance.
* <xref:Microsoft.AspNetCore.Components.QuickGrid.QuickGrid%601.Pagination%2A>: Optionally links this `TGridItem` instance with a <xref:Microsoft.AspNetCore.Components.QuickGrid.PaginationState> model, causing the grid to fetch and render only the current page of data. This is normally used in conjunction with a <xref:Microsoft.AspNetCore.Components.QuickGrid.Paginator> component or some other UI logic that displays and updates the supplied <xref:Microsoft.AspNetCore.Components.QuickGrid.PaginationState> instance.
* In the QuickGrid child content (<xref:Microsoft.AspNetCore.Components.RenderFragment>), specify <xref:Microsoft.AspNetCore.Components.QuickGrid.PropertyColumn`2>s, which represent `TGridItem` columns whose cells display values:
  * <xref:Microsoft.AspNetCore.Components.QuickGrid.PropertyColumn%602.Property%2A>: Defines the value to be displayed in this column's cells.
  * <xref:Microsoft.AspNetCore.Components.QuickGrid.PropertyColumn%602.Format%2A>: Optionally specifies a format string for the value. Using <xref:Microsoft.AspNetCore.Components.QuickGrid.PropertyColumn%602.Format%2A> requires the `TProp` type to implement <xref:System.IFormattable>.
  * <xref:Microsoft.AspNetCore.Components.QuickGrid.ColumnBase%601.Sortable%2A>: Indicates whether the data should be sortable by this column. The default value may vary according to the column type. For example, a <xref:Microsoft.AspNetCore.Components.QuickGrid.TemplateColumn%601> is sorted if any <xref:Microsoft.AspNetCore.Components.QuickGrid.TemplateColumn%601.SortBy%2A> parameter is specified.
  * <xref:Microsoft.AspNetCore.Components.QuickGrid.ColumnBase%601.InitialSortDirection%2A>: Indicates the sort direction if <xref:Microsoft.AspNetCore.Components.QuickGrid.ColumnBase%601.IsDefaultSortColumn%2A> is `true`.
  * <xref:Microsoft.AspNetCore.Components.QuickGrid.ColumnBase%601.IsDefaultSortColumn%2A>: Indicates whether this column should be sorted by default.
  * <xref:Microsoft.AspNetCore.Components.QuickGrid.ColumnBase%601.PlaceholderTemplate%2A>: If specified, virtualized grids use this template to render cells whose data hasn't been loaded.
  * <xref:Microsoft.AspNetCore.Components.QuickGrid.ColumnBase%601.HeaderTemplate>: An optional template for this column's header cell. If not specified, the default header template includes the <xref:Microsoft.AspNetCore.Components.QuickGrid.ColumnBase%601.Title>, along with any applicable sort indicators and options buttons.
  * <xref:Microsoft.AspNetCore.Components.QuickGrid.ColumnBase%601.Title>: Title text for the column. The title is rendered automatically if <xref:Microsoft.AspNetCore.Components.QuickGrid.ColumnBase%601.HeaderTemplate> isn't used.

:::moniker-end

For example, add the following component to render a grid.

For Blazor Web Apps, the `QuickGrid` component must adopt an [interactive render mode](xref:blazor/components/render-modes#render-modes) to enable interactive features, such as paging and sorting.

`PromotionGrid.razor`:

:::moniker range=">= aspnetcore-9.0"

:::code language="razor" source="~/../blazor-samples/9.0/BlazorSample_BlazorWebApp/Components/Pages/PromotionGrid.razor":::

:::moniker-end

:::moniker range="< aspnetcore-9.0"

:::code language="razor" source="~/../blazor-samples/8.0/BlazorSample_BlazorWebApp/Components/Pages/PromotionGrid.razor":::

:::moniker-end

Access the component in a browser at the relative path `/promotion-grid`.

There aren't current plans to extend QuickGrid with features that full-blown commercial grids tend to offer, for example, hierarchical rows, drag-to-reorder columns, or Excel-like range selections. If you require advanced features that you don't wish to develop on your own, continue using third-party grids.

## Sort by column

The `QuickGrid` component can sort items by columns. In Blazor Web Apps, sorting requires the component to adopt an [interactive render mode](xref:blazor/components/render-modes#render-modes).

Add `Sortable="true"` (<xref:Microsoft.AspNetCore.Components.QuickGrid.ColumnBase%601.Sortable%2A>) to the <xref:Microsoft.AspNetCore.Components.QuickGrid.PropertyColumn%602> tag:

```razor
<PropertyColumn Property="..." Sortable="true" />
```

In the running app, sort the QuickGrid column by selecting the rendered column title.

## Page items with a `Paginator` component

The `QuickGrid` component can page data from the data source. In Blazor Web Apps, paging requires the component to adopt an [interactive render mode](xref:blazor/components/render-modes#render-modes).

Add a <xref:Microsoft.AspNetCore.Components.QuickGrid.PaginationState> instance to the component's `@code` block. Set the <xref:Microsoft.AspNetCore.Components.QuickGrid.PaginationState.ItemsPerPage%2A> to the number of items to display per page. In the following example, the instance is named `pagination`, and ten items per page is set:

```csharp
PaginationState pagination = new PaginationState { ItemsPerPage = 10 };
```

Set the `QuickGrid` component's <xref:Microsoft.AspNetCore.Components.QuickGrid.QuickGrid`1.Pagination> property to `pagination`:

```razor
<QuickGrid Items="..." Pagination="pagination">
```

<!-- UPDATE 10.0 Tracked by https://github.com/dotnet/aspnetcore/issues/57289
                 for multiple paginator components problem. -->

To provide a UI for pagination, add a [`Paginator` component](xref:Microsoft.AspNetCore.Components.QuickGrid.Paginator) above or below the `QuickGrid` component. Set the <xref:Microsoft.AspNetCore.Components.QuickGrid.Paginator.State%2A?displayProperty=nameWithType> to `pagination`:

```razor
<Paginator State="pagination" />
```

In the running app, page through the items using a rendered `Paginator` component.

QuickGrid renders additional empty rows to fill in the final page of data when used with a `Paginator` component. In .NET 9 or later, empty data cells (`<td></td>`) are added to the empty rows. The empty rows are intended to facilitate rendering the QuickGrid with stable row height and styling across all pages.

## Apply row styles

Apply styles to rows using [CSS isolation](xref:blazor/components/css-isolation), which can include styling empty rows for `QuickGrid` components that [page data with a `Paginator` component](#page-items-with-a-paginator-component).

Wrap the `QuickGrid` component in a wrapper block element, for example a `<div>`:

```diff
+ <div>
    <QuickGrid ...>
        ...
    </QuickGrid>
+ </div>
```

Apply a row style with the `::deep` [pseudo-element](https://developer.mozilla.org/docs/Web/CSS/Pseudo-elements). In the following example, row height is set to `2em`, including for empty data rows.

`{COMPONENT}.razor.css`:

```css
::deep tr {
    height: 2em;
}
```

Alternatively, use the following CSS styling approach:

* Display row cells populated with data.
* Don't display empty row cells, which avoids empty row cell borders from rendering per Bootstrap styling.

`{COMPONENT}.razor.css`:

```css
::deep tr:has(> td:not(:empty)) > td {
    display: table-cell;
}

::deep td:empty {
    display: none;
}
```

For more information on using `::deep` [pseudo-elements](https://developer.mozilla.org/docs/Web/CSS/Pseudo-elements) with CSS isolation, see <xref:blazor/components/css-isolation#child-component-support>.

## Custom attributes and styles

QuickGrid also supports passing custom attributes and style classes (<xref:Microsoft.AspNetCore.Components.QuickGrid.QuickGrid%601.Class%2A>) to the rendered table element:

```razor
<QuickGrid Items="..." custom-attribute="value" Class="custom-class">
```

:::moniker range=">= aspnetcore-10.0"

## Style a table row based on the row item

<!-- UPDATE 10.0 API cross-link -->

Apply a stylesheet class to a row of the grid based on the row item using the `RowClass` parameter.

In the following example:

* A row item is represented by the `Person` [record](/dotnet/csharp/language-reference/builtin-types/record). The `Person` record includes a `FirstName` property.
* The `GetRowCssClass` method applies the `highlight-row` class styles to any row where the person's first name is "`Julie`."

```razor
<QuickGrid ... RowClass="GetRowCssClass">
    ...
</QuickGrid>

@code {
    private record Person(int PersonId, string FirstName, string LastName);

    private string GetRowCssClass(Person person) =>
        person.FirstName == "Julie" ? "highlight-row" : null;
}
```

:::moniker-end


:::moniker range=">= aspnetcore-10.0"

<!-- UPDATE 10.0 - API cross-link -->

:::moniker-end

### Close `QuickGrid` column options

Close the `QuickGrid` column options UI with the `HideColumnOptionsAsync` method.

The following example closes the column options UI as soon as the title filter is applied:

```razor
<QuickGrid @ref="movieGrid" Items="movies">
    <PropertyColumn Property="@(m => m.Title)" Title="Title">
        <ColumnOptions>
            <input type="search" @bind="titleFilter" placeholder="Filter by title" 
                @bind:after="@(() => movieGrid.HideColumnOptionsAsync())" />
        </ColumnOptions>
    </PropertyColumn>
    <PropertyColumn Property="@(m => m.Genre)" Title="Genre" />
    <PropertyColumn Property="@(m => m.ReleaseYear)" Title="Release Year" />
</QuickGrid>

@code {
    private QuickGrid<Movie>? movieGrid;
    private string titleFilter = string.Empty;
    private IQueryable<Movie> movies = new List<Movie> { ... }.AsQueryable();
    private IQueryable<Movie> filteredMovies => 
        movies.Where(m => m.Title!.Contains(titleFilter));
}
```

## Entity Framework Core (EF Core) data source

Use the factory pattern to resolve an EF Core database context that provides data to a `QuickGrid` component. For more information on why the factory pattern is recommended, see <xref:blazor/blazor-ef-core>.

A database context factory (<xref:Microsoft.EntityFrameworkCore.IDbContextFactory%601>) is injected into the component with the `@inject` directive. The factory approach requires disposal of the database context, so the component implements the <xref:System.IAsyncDisposable> interface with the `@implements` directive. The item provider for the `QuickGrid` component is a `DbSet<T>` obtained from the created database context (<xref:Microsoft.EntityFrameworkCore.IDbContextFactory%601.CreateDbContext%2A>) of the injected database context factory.

QuickGrid recognizes EF-supplied <xref:System.Linq.IQueryable> instances and knows how to resolve queries asynchronously for efficiency.

Add a package reference for the [`Microsoft.AspNetCore.Components.QuickGrid.EntityFrameworkAdapter` NuGet package](https://www.nuget.org/packages/Microsoft.AspNetCore.Components.QuickGrid.EntityFrameworkAdapter).

[!INCLUDE[](~/includes/package-reference.md)]

Call <xref:Microsoft.Extensions.DependencyInjection.EntityFrameworkAdapterServiceCollectionExtensions.AddQuickGridEntityFrameworkAdapter%2A> on the service collection in the `Program` file to register an EF-aware <xref:Microsoft.AspNetCore.Components.QuickGrid.IAsyncQueryExecutor> implementation:

```csharp
builder.Services.AddQuickGridEntityFrameworkAdapter();
```

The following example uses an `ExampleTable` <xref:Microsoft.EntityFrameworkCore.DbSet%601> (table) from a `AppDbContext` database context (`context`) as the data source for a `QuickGrid` component:

```razor
@using Microsoft.AspNetCore.Components.QuickGrid
@using Microsoft.EntityFrameworkCore
@implements IAsyncDisposable
@inject IDbContextFactory<AppDbContext> DbFactory

...

<QuickGrid ... Items="context.ExampleTable" ...>
    ...
</QuickGrid>

@code {
    private AppDbContext context = default!;

    protected override void OnInitialized()
    {
        context = DbFactory.CreateDbContext();
    }

    public async ValueTask DisposeAsync() => await context.DisposeAsync();
}
```

In the code block (`@code`) of the preceding example:

* The `context` field holds the database context, typed as an `AppDbContext`.
* The `OnInitialized` lifecycle method assigns a new database context (<xref:Microsoft.EntityFrameworkCore.IDbContextFactory%601.CreateDbContext%2A>) to the `context` field from the injected factory (`DbFactory`).
* The asynchronous `DisposeAsync` method disposes of the database context when the component is disposed.

You may also use any EF-supported LINQ operator to filter the data before passing it to the <xref:Microsoft.AspNetCore.Components.QuickGrid.QuickGrid%601.Items%2A> parameter.

The following example filters movies by a movie title entered in a search box. The database context is `BlazorWebAppMoviesContext`, and the model is `Movie`. The movie's `Title` property is used for the filter operation.

```razor
@using Microsoft.AspNetCore.Components.QuickGrid
@using Microsoft.EntityFrameworkCore
@implements IAsyncDisposable
@inject IDbContextFactory<BlazorWebAppMoviesContext> DbFactory

...

<p>
    <input type="search" @bind="titleFilter" @bind:event="oninput" />
</p>

<QuickGrid ... Items="FilteredMovies" ...>
    ...
</QuickGrid>

@code {
    private string titleFilter = string.Empty;
    private BlazorWebAppMoviesContext context = default!;

    protected override void OnInitialized()
    {
        context = DbFactory.CreateDbContext();
    }

    private IQueryable<Movie> FilteredMovies => 
        context.Movie.Where(m => m.Title!.Contains(titleFilter));

    public async ValueTask DisposeAsync() => await context.DisposeAsync();
}
```

For a working example, see the following resources:

* [Build a Blazor movie database app tutorial](xref:blazor/tutorials/movie-database-app/index)
* [Blazor movie database sample app](https://github.com/dotnet/blazor-samples): Select the latest version folder in the repository. The sample folder for the tutorial's project is named `BlazorWebAppMovies`.

## Display name support

A column title can be assigned using <xref:Microsoft.AspNetCore.Components.QuickGrid.ColumnBase%601.Title?displayProperty=nameWithType> in the <xref:Microsoft.AspNetCore.Components.QuickGrid.PropertyColumn`2>'s tag. In the following movie example, the column is given the name "`Release Date`" for the column's movie release date data:

```razor
<PropertyColumn Property="movie => movie.ReleaseDate" Title="Release Date" />
```

However, managing column titles (names) from bound model properties is usually a better choice for maintaining an app. A model can control the display name of a property with the [`[Display]` attribute](xref:System.ComponentModel.DataAnnotations.DisplayAttribute). In the following example, the model specifies a movie release date display name of "`Release Date`" for its `ReleaseDate` property:

```csharp
[Display(Name = "Release Date")]
public DateTime ReleaseDate { get; set; }
```

To enable the `QuickGrid` component to use the <xref:System.ComponentModel.DataAnnotations.DisplayAttribute.Name?displayProperty=nameWithType> property, subclass <xref:Microsoft.AspNetCore.Components.QuickGrid.PropertyColumn`2>, either in the component or in a separate class. Call the <xref:System.ComponentModel.DataAnnotations.DisplayAttribute.GetName%2A> method to return the localized <xref:System.ComponentModel.DataAnnotations.DisplayAttribute.Name?displayProperty=nameWithType> value if an unlocalized <xref:System.ComponentModel.DisplayNameAttribute.DisplayName> ([`[DisplayName]` attribute](xref:System.ComponentModel.DisplayNameAttribute)) doesn't hold the value:

```csharp
public class DisplayNameColumn<TGridItem, TProp> : PropertyColumn<TGridItem, TProp>
{
    protected override void OnParametersSet()
    {
        if (Title is null && Property.Body is MemberExpression memberExpression)
        {
            var memberInfo = memberExpression.Member;
            Title = 
                memberInfo.GetCustomAttribute<DisplayNameAttribute>().DisplayName ??
                memberInfo.GetCustomAttribute<DisplayAttribute>().GetName() ??
                memberInfo.Name;
        }

        base.OnParametersSet();
    }
}
```

Use the subclass in the `QuickGrid` component. In the following example, the preceding `DisplayNameColumn` is used. The name "`Release Date`" is provided by the [`[Display]` attribute](xref:System.ComponentModel.DataAnnotations.DisplayAttribute) in the model, so there's no need to specify a <xref:Microsoft.AspNetCore.Components.QuickGrid.ColumnBase%601.Title>:

```razor
<DisplayNameColumn Property="movie => movie.ReleaseDate" />
```

The [`[DisplayName]` attribute](xref:System.ComponentModel.DisplayNameAttribute) is also supported:

```csharp
[DisplayName("Release Date")]
public DateTime ReleaseDate { get; set; }
```

However, the `[Display]` attribute is recommended because it makes additional properties available. For example, the `[Display]` attribute offers the ability to assign a resource type for localization.

## Remote data

In Blazor WebAssembly apps, fetching data from a JSON-based web API on a server is a common requirement. To fetch only the data that's required for the current page/viewport of data and apply sorting or filtering rules on the server, use the <xref:Microsoft.AspNetCore.Components.QuickGrid.QuickGrid%601.ItemsProvider%2A> parameter.

<xref:Microsoft.AspNetCore.Components.QuickGrid.QuickGrid%601.ItemsProvider%2A> can also be used in a server-side Blazor app if the app is required to query an external endpoint or in other cases where the requirements aren't covered by an <xref:System.Linq.IQueryable>.

Supply a callback matching the <xref:Microsoft.AspNetCore.Components.QuickGrid.GridItemsProvider%601> delegate type, where `TGridItem` is the type of data displayed in the grid. The callback is given a parameter of type <xref:Microsoft.AspNetCore.Components.QuickGrid.GridItemsProviderRequest%601>, which specifies the start index, maximum row count, and sort order of data to return. In addition to returning the matching items, a total item count (`totalItemCount`) is also required for paging and virtualization to function correctly.

The following example obtains data from the public [OpenFDA Food Enforcement database](https://open.fda.gov/apis/food/enforcement/).

The <xref:Microsoft.AspNetCore.Components.QuickGrid.GridItemsProvider%601> converts the <xref:Microsoft.AspNetCore.Components.QuickGrid.GridItemsProviderRequest%601> into a query against the OpenFDA database. Query parameters are translated into the particular URL format supported by the external JSON API. It's only possible to perform sorting and filtering via sorting and filtering that's supported by the external API. The OpenFDA endpoint doesn't support sorting, so none of the columns are marked as sortable. However, it does support skipping records (`skip` parameter) and limiting the return of records (`limit` parameter), so the component can enable virtualization and scroll quickly through tens of thousands of records.

`FoodRecalls.razor`:

```razor
@page "/food-recalls"
@inject HttpClient Http
@inject NavigationManager Navigation

<PageTitle>Food Recalls</PageTitle>

<h1>OpenFDA Food Recalls</h1>

<div class="grid" tabindex="-1">
    <QuickGrid ItemsProvider="@foodRecallProvider" Virtualize="true">
        <PropertyColumn Title="ID" Property="@(c => c.Event_Id)" />
        <PropertyColumn Property="@(c => c.State)" />
        <PropertyColumn Property="@(c => c.City)" />
        <PropertyColumn Title="Company" Property="@(c => c.Recalling_Firm)" />
        <PropertyColumn Property="@(c => c.Status)" />
    </QuickGrid>
</div>

<p>Total: <strong>@numResults results found</strong></p>

@code {
    private GridItemsProvider<FoodRecall>? foodRecallProvider;
    private int numResults;

    protected override async Task OnInitializedAsync()
    {
        foodRecallProvider = async req =>
        {
            var url = Navigation.GetUriWithQueryParameters(
                "https://api.fda.gov/food/enforcement.json", 
                new Dictionary<string, object?>
            {
                { "skip", req.StartIndex },
                { "limit", req.Count },
            });

            using var response = await Http.GetFromJsonAsync<FoodRecallQueryResult>(
                url, req.CancellationToken);

            return GridItemsProviderResult.From(
                items: response!.Results,
                totalItemCount: response!.Meta.Results.Total);
        };

        numResults = (await Http.GetFromJsonAsync<FoodRecallQueryResult>(
            "https://api.fda.gov/food/enforcement.json"))!.Meta.Results.Total;
    }
}
```

For more information on calling web APIs, see <xref:blazor/call-web-api>.

## QuickGrid scaffolder

The QuickGrid scaffolder scaffolds Razor components with QuickGrid to display data from a database.

The scaffolder generates basic Create, Read, Update, and Delete (CRUD) pages based on an Entity Framework Core data model. You can scaffold individual pages or all of the CRUD pages. You select the model class and the `DbContext`, optionally creating a new `DbContext` if needed.

The scaffolded Razor components are added to the project's in a generated folder named after the model class. The generated `Index` component uses a `QuickGrid` component to display the data. Customize the generated components as needed and enable interactivity to take advantage of interactive features, such as [paging](#page-items-with-a-paginator-component), [sorting](#sort-by-column) and filtering.

The components produced by the scaffolder require server-side rendering (SSR), so they aren't supported when running on WebAssembly.

# [Visual Studio](#tab/visual-studio)

Right-click on the `Components/Pages` folder and select **Add** > **New Scaffolded Item**.

With the **Add New Scaffold Item** dialog open to **Installed** > **Common** > **Blazor** > **Razor Component**, select **Razor Components using Entity Framework (CRUD)**. Select the **Add** button.

*CRUD* is an acronym for Create, Read, Update, and Delete. The scaffolder produces create, edit, delete, details, and index components for the app.

Complete the **Add Razor Components using Entity Framework (CRUD)** dialog:

<!-- UPDATE 10.0 Keep an eye on https://github.com/dotnet/Scaffolding/issues/3131
                 for a scaffolding update that makes the scaffolder a little 
                 smarter about suggesting an existing dB context that has a
                 factory provider. If the scaffolder can do that, this instruction
                 can be lightened up to just select ANY one from the list of 
                 existing providers or create a new one. -->

* The **Template** dropdown list includes other templates for specifically creating create, edit, delete, details, and list components. This dropdown list comes in handy when you only need to create a specific type of component scaffolded to a model class. Leave the **Template** dropdown list set to **CRUD** to scaffold a full set of components.
* In the **Model class** dropdown list, select the model class. A folder is created for the generated components from the model name (if the model class is named `Movie`, the folder is automatically named `MoviePages`).
* For **DbContext class**, take either of the following approaches:
  * Select an existing <xref:Microsoft.EntityFrameworkCore.DbContext> class that you know has a factory provider registration (<xref:Microsoft.Extensions.DependencyInjection.EntityFrameworkServiceCollectionExtensions.AddDbContextFactory%2A>).
  * Select the **+** (plus sign) button and use the **Add Data Context** modal dialog to supply a new <xref:Microsoft.EntityFrameworkCore.DbContext> class name, which registers the class with a factory provider instead of using the context type directly as a service registration.
* After the model dialog closes, the **Database provider** dropdown list defaults to **SQL Server**. You can select the appropriate provider for the database that you're using. The options include SQL Server, SQLite, PostgreSQL, and Azure Cosmos DB.
* Select **Add**.

# [Visual Studio Code](#tab/visual-studio-code)

Paste all of the following commands at the prompt (`>`) of the **Terminal** (**Terminal** menu > **New Terminal**) opened to the project's root directory. When you paste multiple commands, a warning appears stating that multiple commands will execute. Dismiss the warning and proceed with the paste operation.

When you paste multiple commands, all of the commands execute except the last one. The last command doesn't execute until you press <kbd>Enter</kbd> on the keyboard.

```dotnetcli
dotnet tool install --global dotnet-aspnet-codegenerator
dotnet tool install --global dotnet-ef
dotnet add package Microsoft.EntityFrameworkCore.SQLite
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet add package Microsoft.EntityFrameworkCore.SqlServer
dotnet add package Microsoft.EntityFrameworkCore.Tools
dotnet add package Microsoft.AspNetCore.Components.QuickGrid
dotnet add package Microsoft.AspNetCore.Components.QuickGrid.EntityFrameworkAdapter
```

> [!IMPORTANT]
> After the first eight commands execute, make sure that you press <kbd>Enter</kbd> on the keyboard to execute the last command.

The preceding commands add:

* [Command-line interface (CLI) tools for EF Core](/ef/core/miscellaneous/cli/dotnet)
* [`aspnet-codegenerator` scaffolding tool](xref:fundamentals/tools/dotnet-aspnet-codegenerator)
* Design time tools for EF Core
* The SQLite and SQL Server providers with the EF Core package as a dependency
* [`Microsoft.VisualStudio.Web.CodeGeneration.Design`](https://www.nuget.org/packages/Microsoft.VisualStudio.Web.CodeGeneration.Design) for scaffolding

In the **Terminal**, execute the following command to scaffold a full set of components with the `CRUD` template:

```dotnetcli
dotnet aspnet-codegenerator blazor CRUD -dbProvider {PROVIDER} -dc {DB CONTEXT CLASS} -m {MODEL} -outDir {PATH}
```

> [!NOTE]
> The preceding command is a .NET CLI command, and .NET CLI commands are executed when entered at a [PowerShell](/powershell/) prompt, which is the default command shell of the VS Code **Terminal**.

The following table explains the ASP.NET Core code generator options in the preceding command.

Option        | Placeholder          | Description
------------- | -------------------- | ---
`-dbProvider` | `{PROVIDER}`         | Database provider to use. Options include `sqlserver` (default), `sqlite`, `cosmos`, `postgres`.
`-dc`         | `{DB CONTEXT CLASS}` | The <xref:Microsoft.EntityFrameworkCore.DbContext> class to use, including the namespace. To make sure that a <xref:Microsoft.EntityFrameworkCore.DbContext> class is registered in the app's services with a factory provider (<xref:Microsoft.Extensions.DependencyInjection.EntityFrameworkServiceCollectionExtensions.AddDbContextFactory%2A>), either use an existing <xref:Microsoft.EntityFrameworkCore.DbContext> class that you know has a factory provider registration or supply a new <xref:Microsoft.EntityFrameworkCore.DbContext> class.
`-m`          | `{MODEL}`            | The name of the model class.
`-outDir`     | `{PATH}`             | The output directory for the generated components. A folder is created from the model name in the output directory to hold the components (if the model class is named `Movie`, the folder is automatically named `MoviePages`). The path is typically either `Components/Pages` for a Blazor Web App or `Pages` for a standalone Blazor WebAssembly app.

For the additional Blazor provider options, use the .NET CLI help option (`-h`|`--help`):

```dotnetcli
dotnet aspnet-codegenerator blazor -h
```

# [.NET CLI](#tab/net-cli)

Paste all of the following commands at the prompt (`>`) of a command shell opened to the project's root directory. When you paste multiple commands, a warning appears stating that multiple commands will execute. Dismiss the warning and proceed with the paste operation.

When you paste multiple commands, all of the commands execute except the last one. The last command doesn't execute until you press <kbd>Enter</kbd> on the keyboard.

```dotnetcli
dotnet tool install --global dotnet-aspnet-codegenerator
dotnet tool install --global dotnet-ef
dotnet add package Microsoft.EntityFrameworkCore.SQLite
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet add package Microsoft.EntityFrameworkCore.SqlServer
dotnet add package Microsoft.EntityFrameworkCore.Tools
dotnet add package Microsoft.AspNetCore.Components.QuickGrid
dotnet add package Microsoft.AspNetCore.Components.QuickGrid.EntityFrameworkAdapter
```

> [!IMPORTANT]
> After the first eight commands execute, make sure that you press <kbd>Enter</kbd> on the keyboard to execute the last command.

The preceding commands add:

* [Command-line interface (CLI) tools for EF Core](/ef/core/miscellaneous/cli/dotnet)
* [`aspnet-codegenerator` scaffolding tool](xref:fundamentals/tools/dotnet-aspnet-codegenerator)
* Design time tools for EF Core
* The SQLite and SQL Server providers with the EF Core package as a dependency
* [`Microsoft.VisualStudio.Web.CodeGeneration.Design`](https://www.nuget.org/packages/Microsoft.VisualStudio.Web.CodeGeneration.Design) for scaffolding.

In a command shell, execute the following command to scaffold a full set of components with the `CRUD` template:

```dotnetcli
dotnet aspnet-codegenerator blazor CRUD -dbProvider {PROVIDER} -dc {DB CONTEXT CLASS} -m {MODEL} -outDir {PATH}
```

The following table explains the ASP.NET Core code generator options in the preceding command.

Option        | Placeholder          | Description
------------- | -------------------- | ---
`-dbProvider` | `{PROVIDER}`         | Database provider to use. Options include `sqlserver` (default), `sqlite`, `cosmos`, `postgres`.
`-dc`         | `{DB CONTEXT CLASS}` | The <xref:Microsoft.EntityFrameworkCore.DbContext> class to use, including the namespace. To make sure that a <xref:Microsoft.EntityFrameworkCore.DbContext> class is registered in the app's services with a factory provider (<xref:Microsoft.Extensions.DependencyInjection.EntityFrameworkServiceCollectionExtensions.AddDbContextFactory%2A>), either use an existing <xref:Microsoft.EntityFrameworkCore.DbContext> class that you know has a factory provider registration or supply a new <xref:Microsoft.EntityFrameworkCore.DbContext> class.
`-m`          | `{MODEL}`            | The name of the model class.
`-outDir`     | `{PATH}`             | The output directory for the generated components. A folder is created from the model name in the output directory to hold the components (if the model class is named `Movie`, the folder is automatically named `MoviePages`). The path is typically either `Components/Pages` for a Blazor Web App or `Pages` for a standalone Blazor WebAssembly app.

For the additional Blazor provider options, use the .NET CLI help option (`-h`|`--help`):

```dotnetcli
dotnet aspnet-codegenerator blazor -h
```

---

For an example use of the QuickGrid scaffolder, see <xref:blazor/tutorials/movie-database-app/index>.

<!-- UPDATE 11.0 - PU work tracked by https://github.com/dotnet/aspnetcore/issues/58716.
                   We will continue to show this for now. The PU plans to look at it
                   for framework updates at 11.0. -->

## Multiple concurrent EF Core queries trigger `System.InvalidOperationException`

Multiple concurrent EF Core queries can trigger the following <xref:System.InvalidOperationException?displayProperty=fullName>:

> :::no-loc text="System.InvalidOperationException: A second operation was started on this context instance before a previous operation completed. This is usually caused by different threads concurrently using the same instance of DbContext. For more information on how to avoid threading issues with DbContext, see https://go.microsoft.com/fwlink/?linkid=2097913.":::

This scenario is scheduled for improvement in an upcoming release of ASP.NET Core. For more information, see [[Blazor] Improve the experience with QuickGrid and EF Core (`dotnet/aspnetcore` #58716)](https://github.com/dotnet/aspnetcore/issues/58716).

In the meantime, you can address the problem using an <xref:Microsoft.AspNetCore.Components.QuickGrid.QuickGrid%601.ItemsProvider%2A> with a cancellation token. The cancellation token prevents concurrent queries by cancelling the previous request when a new request is issued.

Consider the following example, which is based on the movie database `Index` component for the <xref:blazor/tutorials/movie-database-app/index> tutorial. The simpler version scaffolded into the app can be seen in the article's [sample app](xref:blazor/tutorials/movie-database-app/index#sample-app). The `Index` component scaffolded into the app is replaced by the following component.

`Components/Pages/MoviePages/Index.razor`:

```razor
@page "/movies"
@rendermode InteractiveServer
@using Microsoft.EntityFrameworkCore
@using Microsoft.AspNetCore.Components.QuickGrid
@using BlazorWebAppMovies.Models
@using BlazorWebAppMovies.Data
@inject IDbContextFactory<BlazorWebAppMovies.Data.BlazorWebAppMoviesContext> DbFactory

<PageTitle>Index</PageTitle>

<h1>Index</h1>

<div>
    <input type="search" @bind="titleFilter" @bind:event="oninput" />
</div>

<p>
    <a href="movies/create">Create New</a>
</p>

<div>
    <QuickGrid Class="table" TGridItem="Movie" ItemsProvider="GetMovies"
            ItemKey="(x => x.Id)" Pagination="pagination">
        <PropertyColumn Property="movie => movie.Title" Sortable="true" />
        <PropertyColumn Property="movie => movie.ReleaseDate" Title="Release Date" />
        <PropertyColumn Property="movie => movie.Genre" />
        <PropertyColumn Property="movie => movie.Price" />
        <PropertyColumn Property="movie => movie.Rating" />

        <TemplateColumn Context="movie">
            <a href="@($"movies/edit?id={movie.Id}")">Edit</a> |
            <a href="@($"movies/details?id={movie.Id}")">Details</a> |
            <a href="@($"movies/delete?id={movie.Id}")">Delete</a>
        </TemplateColumn>
    </QuickGrid>
</div>

<Paginator State="pagination" />

@code {
    private BlazorWebAppMoviesContext context = default!;
    private PaginationState pagination = new PaginationState { ItemsPerPage = 5 };
    private string titleFilter = string.Empty;

    public async ValueTask<GridItemsProviderResult<Movie>> GetMovies(GridItemsProviderRequest<Movie> request)
    {
        using var context = DbFactory.CreateDbContext();
        var totalCount = await context.Movie.CountAsync(request.CancellationToken);
        IQueryable<Movie> query = context.Movie.OrderBy(x => x.Id);
        query = request.ApplySorting(query).Skip(request.StartIndex);

        if (request.Count.HasValue)
        {
            query = query.Take(request.Count.Value);
        }

        var items = await query.ToArrayAsync(request.CancellationToken);

        var result = new GridItemsProviderResult<Movie>
        {
            Items = items,
            TotalItemCount = totalCount
        };

        return result;
    }
}
```
