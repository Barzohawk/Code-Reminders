Telerik & .NET Code Snippets


### Custom Grid Data Filtering (Telerik)
**Summary:** Implements custom filtering logic for Telerik Grid controls in .NET applications.

```csharp
public List<ExampleModel_1> ApplyCustomFilter(List<ExampleModel_1> data, string filterValue)
{
    return data.Where(item => item.ExampleProperty_1.Contains(filterValue)).ToList();
}
```

---

### API Data Fetching in .NET
**Summary:** Fetches data from an external API and parses JSON response.

```csharp
public async Task<List<ExampleModel_2>> FetchDataFromAPI()
{
    using (HttpClient client = new HttpClient())
    {
        string apiUrl = "https://api.example.com/example_endpoint_1";
        HttpResponseMessage response = await client.GetAsync(apiUrl);
        if (response.IsSuccessStatusCode)
        {
            string jsonData = await response.Content.ReadAsStringAsync();
            return JsonConvert.DeserializeObject<List<ExampleModel_2>>(jsonData);
        }
    }
    return new List<ExampleModel_2>();
}
```

---

### Event Handling in Telerik UI
**Summary:** Handles user interactions with Telerik UI components in ASP.NET applications.

```csharp
protected void ExampleButton_Click(object sender, EventArgs e)
{
    ExampleLabel_1.Text = "Button clicked!";
}
```

---



### Custom Grid Stylesheet Injection (Telerik)
**Summary:** Disables default Telerik stylesheets and injects a custom grid stylesheet.

***.Net Framework(V4.5-4.8) - ASP.NET Web Forms***

Used in Web Forms applications to manipulate the page header dynamically.

```csharp
protected void Page_PreRender(object sender, EventArgs e)
{
    foreach (Control control in this.Page.Header.Controls)
    {
        if (control is HtmlLink link && link.Href.Contains("Telerik.Web.UI.Skins"))
        {
            this.Page.Header.Controls.Remove(control);
        }
    }
    HtmlLink customStylesheet = new HtmlLink();
    customStylesheet.Href = "/example_styles_1.css";
    customStylesheet.Attributes["rel"] = "stylesheet";
    this.Page.Header.Controls.Add(customStylesheet);
}
```
***.NET Core(ASP.NET Core MVC)*** 

For applications using ASP.NET Core MVC, styles should be handled in the _Layout.cshtml file instead of manipulating headers

```csharp
@{
    // Remove default Telerik styles dynamically if necessary
    ViewData["CustomStyles"] = "/example_styles_1.css";
}

<link rel="stylesheet" href="@ViewData["CustomStyles"]" />
```


---

### Moving the Telerik Grid Navigation Bar
**Summary:** Relocates the built-in **Telerik Grid navigation bar** to a custom location in **Web Forms** and **ASP.NET Core (MVC)**.

##### **For ASP.NET Web Forms (.NET Framework 4.5 - 4.8)**
Used in **Web Forms applications**, this approach detaches the built-in pager and places it elsewhere.

```csharp
protected void Page_Load(object sender, EventArgs e)
{
    if (!IsPostBack)
    {
        MoveTelerikGridPager();
    }
}

private void MoveTelerikGridPager()
{
    GridPagerItem pagerItem = ExampleTelerikGrid_1.MasterTableView.GetItems(GridItemType.Pager).FirstOrDefault() as GridPagerItem;
    if (pagerItem != null)
    {
        ExampleCustomPagerContainer_1.Controls.Add(pagerItem);
    }
}
```

##### **For ASP.NET Core (MVC)**
Instead of manipulating server-side controls, Telerik for **ASP.NET Core** uses the `DataBound` event to detach and move the pager.

```javascript
function onDataBound(e) {
    var pager = $(".k-pager-wrap");
    if (pager.length) {
        $("#example_custom_pager_1").append(pager);
    }
}
```

Attach the event handler in the Grid's configuration:

```html
@(Html.Kendo().Grid<ExampleModel_1>()
    .Name("ExampleTelerikGrid_1")
    .Events(events => events.DataBound("onDataBound"))
)
```






