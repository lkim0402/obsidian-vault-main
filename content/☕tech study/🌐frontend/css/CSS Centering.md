# With margins
1. Be a `display` block
2. Must have a set `width`
3. Margin left/right: auto (both)
	- If the values of `margin-left` and `margin-right` are both `auto`, the calculated space is evenly distributed
# Text align
- Works on all inline elements
```html
<div class="btn-parent">
	<button class="btn">Google Search</button>
	<button class="btn">I'm Feeling Lucky</button>
</div>
```

```css
.btn-parent {
    text-align: center;
}

.btn {
    margin-top:30px;
    display:inline;
    background: #dfe1e5;
    border: none;
    border-radius: 5px;
    padding: 8px;
    padding-left: 13px;
    padding-right: 13px;
}
```
- `text-align` should be applied in the parent

# Flexbox!
- [[CSS Flexbox]]