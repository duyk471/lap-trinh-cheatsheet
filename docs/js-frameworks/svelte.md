# Svelte

### Khởi tạo + thêm vào dự án...
Đang học trên Documentation của dự án...


### Styling
Importantly, these rules are scoped to the component. You won’t accidentally change the style of <p> elements elsewhere in your app, as we’ll see in the next step.


html directly

```html
<script>
	let string = `this string contains some <strong>HTML!!!</strong>`;
</script>

<p>{@html string}</p>
```


### State thường và deep state

As we saw in the previous exercise, state reacts to reassignments. But it also reacts to mutations — we call this deep reactivity.

Often, you will need to derive state from other state. For this, we have the $derived rune:

The expression inside the $derived declaration will be re-evaluated whenever its dependencies (in this case, just numbers) are updated. Unlike normal state, derived state is read-only.