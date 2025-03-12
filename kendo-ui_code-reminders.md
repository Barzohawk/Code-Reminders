# Kendo UI Code Reminders
## Expanding Kendo UI K-Window when using custom collapse-right

I was running into issues with Kendo UI where the frame for the form was creating a scroll when a collapse was opened to the right for additional fields when a button was clicked. this following code solved the problem. Keep in mind, you may not need the .k-edit-form-container like I did. Use as you need. Enjoy!

### k-window Changes

```html
<div class="k-edit-form-container">
  <div class="custom-edit-box">
    <button id="customToggleCollapseBtn" class="k-button" type="button">Collapse Button</button>
    <div id="main">
      content
    </div>
    <div class="collapse custom-collapse" id="customCollapse">
      <div id="secondary">
        content
      </div>
    </div>
  </div>
</div>
```

### CSS

```css
.custom-edit-box {
  padding: 20px;
  border-radius: 5px;
  position: relative;
}
.custom-collapse {
  position: absolute;
  top: 0;
  left: calc(100% + 10px);
  z-index; 10050;
  display: none;
}
```

### JQuery

```jquery
function adjustWindowWidth() {
  var windowWidth = $(window).width();
  var secondaryWidth = $("#secondary").outerWidth(true);
  var mainwidth = $("#main").outerWidth(true);
  var padding = 40;

  var newWindowWidth;

  if ($("#customCollapse").is(":visible")) {
    newWindowWidth = secondaryWidth + mainWidth + padding;
  } else {
    newWindowWidth = mainWidth + padding;
  }

  $(".k-window").width(newWindowWidth);

}

function onEdit(e){
  $("#customToggleCollapseBtn").click(function () {
    $("#customCollapse").toggle();
    adjustWindowWidth();
  });
 }
```

I'm sure you will have to make it your own I just made this public because it was such a pain for myself that I wanted to stash it for later use and to share with anyone else.
