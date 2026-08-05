# Shopify Liquid Snippets

Dependency-free Liquid snippets you can drop straight into any Shopify theme (Online Store 2.0 / Dawn compatible). Every file is self-contained, documented in a comment block at the top, and MIT licensed - copy what you need, delete the rest.

Maintained by [Ecom Swift LLC](https://ecomswiftllc.com).

## Install

1. In Shopify admin go to **Online Store → Themes → ⋯ → Edit code** (or pull the theme with Shopify CLI).
2. Create a new file inside `snippets/` using the same file name.
3. Paste the contents, save, then render it from a section or template.

```liquid
{% render 'price', product: product %}
```

## What is inside

| Snippet | What it does | Render example |
| --- | --- | --- |
| `snippets/price.liquid` | Price block with compare-at price, savings, unit price and tax note | `{% render 'price', product: product %}` |
| `snippets/product-badge.liquid` | Sale % / Sold out / New / Low stock badges for product cards | `{% render 'product-badge', product: product, low_stock_at: 5 %}` |
| `snippets/responsive-image.liquid` | Correct `srcset` + `sizes` + aspect-ratio image, no layout shift | `{% render 'responsive-image', image: product.featured_image, sizes: '50vw' %}` |
| `snippets/product-json-ld.liquid` | Product structured data for Google rich results | `{% render 'product-json-ld', product: product %}` |
| `snippets/pagination.liquid` | Accessible pagination with "showing X-Y of Z" | `{% render 'pagination', paginate: paginate %}` |
| `snippets/free-shipping-bar.liquid` | Free shipping progress bar for cart / cart drawer | `{% render 'free-shipping-bar', threshold: 5000 %}` |
| `snippets/metafield.liquid` | Renders any metafield type safely (text, rich text, rating, file, references, lists, metaobjects) | `{% render 'metafield', field: product.metafields.custom.care %}` |

## Conventions used

- **`render` over `include`.** `include` is deprecated and leaks scope; every snippet here takes explicit parameters.
- **Whitespace control everywhere.** `{%-` / `-%}` keeps the rendered HTML clean, which matters for inline and flex layouts.
- **No jQuery, no build step, no external CSS.** Styles ship in a small `<style>` block so a snippet works the moment you save it. Move them into your theme stylesheet for production.
- **Money in cents.** Shopify stores prices as integers, so `5000` means 50.00 in the shop currency. Always pipe through `| money`.
- **Fail quietly.** Snippets check for `blank` before rendering so an empty metafield or missing image never breaks a page.

## Gotchas worth knowing

- `product.selected_or_first_available_variant` is what you want on a product page; `product.variants.first` will show a sold-out variant.
- `divided_by` does integer division unless one side is a float, so use `100.0` when converting cents to a decimal.
- Structured data should only include `aggregateRating` when reviews actually exist, otherwise Search Console reports an error.
- Anything rendered inside a cart drawer needs re-rendering after a Cart AJAX API call. Pass `sections` in the request body and swap the returned HTML.

## Contributing

Issues and pull requests are welcome. A good snippet PR has a comment header documenting every parameter, uses whitespace control, and works on a stock Dawn theme.

## License

MIT - see [LICENSE](LICENSE). Use it in client work, commercial themes, anywhere.
