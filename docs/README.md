# Wordpress Integration

<!-- ALL-CONTRIBUTORS-BADGE:START - Do not remove or modify this section -->

[![All Contributors](https://img.shields.io/badge/all_contributors-0-orange.svg?style=flat-square)](#contributors-)

<!-- ALL-CONTRIBUTORS-BADGE:END -->

## Description

This app provides a way to bring in blog data from the Wordpress API and create a blog "homepage", category pages, and blog post pages on your VTEX IO store, using your existing store header, footer, and styling.

## Usage

### Setup

After installing this app in your account, navigate to the app's settings in your admin dashboard under **Apps** > **My Apps** > **Wordpress Integration**.

_Wordpress URL_ is required for the integration to function. This should be the domain where the Wordpress API endpoint is hosted and Wordpress is administered.

_Title tag for blog homepage_ will determine the title tag for the Wordpress portions of your store.

All other configuration of Wordpress is done within your store's `store-theme`.

### Store Theme: Dependencies

After installing the app in your account, add it as a `peerDependency` in your `store-theme`:

```json
"peerDependencies": {
  "vtex.wordpress-integration": "2.x"
}
```

### Store Theme: Routes

The Wordpress integration is designed to work with these store pages:

`store.blog-home`: The "homepage" of your blog. This can also be extended (i.e. `store.blog-home#custom`) to create additional custom blog pages, if needed.

`store.blog-category`: A listing of blog posts belonging to a particular category, derived from a category slug in the page route.

`store.blog-post`: A detail view of a single blog post, derived from a post slug in the page route.

`store.blog-search-result`: A listing of blog posts matching a search query, derived from a search term in the page route.

Each of the above pages needs a route declared in your `store-theme`'s `store/routes.json`. These are the recommended routes to use:

```json
"store.blog-home": {
	"path": "/blog"
},
"store.blog-category": {
	"path": "/blog/category/:categoryslug"
},
"store.blog-post": {
	"path": "/blog/post/:slug"
},
"store.blog-search-result": {
	"path": "/blog/search/:term"
}
```

You may change "blog" in each route to another string of your choosing.

If you wish to display WordPress "pages" on your store site in addition to posts, you can add a route like this:

```json
"store.blog-home#page": {
	"path": "/blog/page/:slug"
}
```

**As of version 1.5.0**, routes for `store.blog-home`, `store.blog-category`, and `store.blog-search-result` can now optionally accept `:page` parameters for URL-controlled pagination of the `wordpress-all-posts`, `wordpress-category-list`, and `wordpress-search-list` blocks. For example:

```json
"store.blog-home": {
	"path": "/blog(/page/:page)"
},
"store.blog-category": {
	"path": "/blog/category/:categoryslug(/page/:page)"
},
"store.blog-search-result": {
	"path": "/blog/search/:term(/page/:page)"
},
```

If the `:page` parameter is not provided, pagination can alternatively be controlled by query string, for example `/blog/category/example-category?page=2` .

### Store Theme: Blocks

Once the routes are set up, you may populate each blog page with blocks. The Wordpress Integration app provides the following blocks for your use:

- `blog-all-posts.wordpress-all-posts`: A paginated list of all blog posts, starting with the most recent. Recommended to be placed on `store.blog-home`.

- `blog-category-list.wordpress-category-list`: A paginated list of blog posts from a specific category. This must be placed on the `store.blog-category` page, as the category slug is provided by the parameter in the page route.

- `blog-post-details.wordpress-post-details`: Details for a single blog post. This must be placed on the `store.blog-post` page, as the post slug is provided by the parameter in the page route.

- `blog-page-details.wordpress-page-details` : Details for a single blog page. This must be placed on a customized `store.blog-home#page` page to render the contents via the slug passed through the route parameter

- `blog-latest-posts-preview.wordpress-latest-posts-preview`: A small block showing teasers for the most recent 3-5 posts (default is 3). The following props can be used:

| Prop Name         | Description                                                                                                                                                                                                        | Type    | Default value  |
| ----------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ | ------- | -------------- |
| `title`           | A title to be displayed above the block.                                                                                                                                                                           | String  | (empty string) |
| `numberOfPosts`   | The number of posts to be displayed.                                                                                                                                                                               | Integer | 3              |
| `useTextOverlays` | If true, the information (title, category, etc) for each blog post will be overlaid on the post's featured image. If false, posted date and category are shown above the image, title and excerpt are shown below. | Boolean | false          |
| `showCategories`  | If false, the category of each post will not be shown.                                                                                                                                                             | Boolean | true           |
| `showDates`       | If false, the posted date of each post will not be shown                                                                                                                                                           | Boolean | true           |
| `showAuthors`     | If false, the author of each post will not be shown.                                                                                                                                                               | Boolean | false          |
| `showExcerpts`    | If false, the excerpt of each post will not be shown.                                                                                                                                                              | Boolean | false          |
| `absoluteLinks`    | If true, the links from each blog post will point to the external blog in a new tab.                                                                                                                                                              | Boolean | false         |

- `blog-category-preview.wordpress-category-preview`: A small block showing teasers for the most recent 3-5 posts from a specific category (default is 3). The category ID must be provided as a prop. The following props can be used:

| Prop Name          | Description                                                                                                                                                                                                        | Type    | Default value  |
| ------------------ | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ | ------- | -------------- |
| `category`         | The numeric ID of the category in the WordPress system.                                                                                                                                                            | Integer | 0              |
| `title`            | A title to be displayed above the block.                                                                                                                                                                           | String  | (empty string) |
| `description`      | A "subheader" for the block, to be displayed below the title.                                                                                                                                                      | String  | (empty string) |
| `customLinkText`   | The category block includes a link to the `store.blog-category` page for that category. By default the text for the link is "All [category] Posts >", but you may enter custom link text here.                     | String  | (empty string) |
| `customLinkTarget` | If you would like the aforementioned link to direct the user somewhere other than the `store.blog-category` page, you may enter a different target here.                                                           | String  | (empty string) |
| `numberOfPosts`    | The number of posts to be displayed.                                                                                                                                                                               | Integer | 3              |
| `useTextOverlays`  | If true, the information (title, category, etc) for each blog post will be overlaid on the post's featured image. If false, posted date and category are shown above the image, title and excerpt are shown below. | Boolean | false          |
| `showDates`        | If false, the posted date of each post will not be shown                                                                                                                                                           | Boolean | true           |
| `showAuthors`      | If false, the author of each post will not be shown.                                                                                                                                                               | Boolean | false          |
| `showExcerpts`     | If false, the excerpt of each post will not be shown.                                                                                                                                                              | Boolean | false          |
| `absoluteLinks`    | If true, the links from each blog post will point to the external blog in a new tab.                                                                                                                                                              | Boolean | false         |

- `blog-search.wordpress-search`: A search box that shoppers can use to search blog articles. When submitted, the shopper is redirected to the `store.blog-search-list` page. The following prop may be used:

| Prop Name     | Description                                                                                                                    | Type   | Default value  |
| ------------- | ------------------------------------------------------------------------------------------------------------------------------ | ------ | -------------- |
| `placeholder` | By default the search block will use "Search articles..." as the input placeholder, but you may enter a different string here. | String | (empty string) |

- `blog-search-list.wordpress-search-list`: A paginated list of blog post search results. This must be placed on the `store.blog-search` page, as the search terms are provided by the parameter in the page route.

- `search-blog-articles-preview.wordpress`: A small block showing 3-5 blog post search results with a link to `store.blog-search-list` for full blog post search results. This can only be placed on the main `store.search-result` page and uses the same search query as the product search component on that page. The following props can be used:

| Prop Name         | Description                                                                                                                                                                                                        | Type    | Default value |
| ----------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ | ------- | ------------- |
| `numberOfPosts`   | The number of posts to be displayed.                                                                                                                                                                               | Integer | 3             |
| `useTextOverlays` | If true, the information (title, category, etc) for each blog post will be overlaid on the post's featured image. If false, posted date and category are shown above the image, title and excerpt are shown below. | Boolean | false         |
| `showCategories`  | If false, the category of each post will not be shown.                                                                                                                                                             | Boolean | true          |
| `showDates`       | If false, the posted date of each post will not be shown                                                                                                                                                           | Boolean | true          |
| `showAuthors`     | If false, the author of each post will not be shown.                                                                                                                                                               | Boolean | false         |
| `showExcerpts`    | If false, the excerpt of each post will not be shown.                                                                                                                                                              | Boolean | false         |
| `absoluteLinks`    | If true, the links from each blog post will point to the external blog in a new tab.                                                                                                                                                              | Boolean | false         |

- `search-blog-articles-list.wordpress`: An alternative to the previous block. This block shows a complete paginated list of blog post search results, similar to `blog-search-list.wordpress-search-list`, but designed to be placed on the `store.search-result` product search page. It automatically uses the same search query as the product search component on that page.

- `blog-related-products.wordpress-related-products`: A specialized wrapper for a product shelf that can be placed on the `store.blog-post` page. This allows you to tag Wordpress posts with product reference codes, and the products in question will then be displayed in the shelf. The tags must be in the format **prod-[reference code]**. For example, if your product had a reference code of "VTEX01", the tag should be "prod-VTEX01". This block must have `product-summary.shelf` as a child.

- `blog-related-posts.wordpress-related-posts`: Similar to the above, but the reverse: this is a block intended to be placed as a child of `store.product` which will show teasers for 3-5 blog posts (3 by default) that are tagged with the reference code of the product being viewed. The following props can be used:

| Prop Name         | Description                                                                                                                                                                                                        | Type    | Default value  |
| ----------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ | ------- | -------------- |
| `title`           | A title to be displayed above the block.                                                                                                                                                                           | String  | (empty string) |
| `numberOfPosts`   | The number of posts to be displayed.                                                                                                                                                                               | Integer | 3              |
| `useTextOverlays` | If true, the information (title, category, etc) for each blog post will be overlaid on the post's featured image. If false, posted date and category are shown above the image, title and excerpt are shown below. | Boolean | false          |
| `showCategories`  | If false, the category of each post will not be shown.                                                                                                                                                             | Boolean | true           |
| `showDates`       | If false, the posted date of each post will not be shown                                                                                                                                                           | Boolean | true           |
| `showAuthors`     | If false, the author of each post will not be shown.                                                                                                                                                               | Boolean | false          |
| `showExcerpts`    | If false, the excerpt of each post will not be shown.                                                                                                                                                              | Boolean | false          |
| `absoluteLinks`    | If true, the links from each blog post will point to the external blog in a new tab.                                                                                                                                                              | Boolean | false         |

- `blog-breadcrumb.wordpress-breadcrumb`: A breadcrumb component intended to be placed at the top of each blog page.

- `blog-search-list.wordpress-category-related-posts`: A block that can be used to display the title and body of one or more posts on a store category or department page. Use case: SEO text for your store's departments. By default, when placed on a store category or department page, the block will attempt to display a WordPress post tagged `"category-{id}"`, where `{id}` is the numeric ID of your VTEX store department. The block accepts the following optional props:

| Prop Name            | Description                                                                                                                                                                    | Type    | Default value  |
| -------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ | ------- | -------------- |
| `categoryIdentifier` | You may manually specify the ID to be used in the WordPress tag. For example, if you set this prop to "test", the block will look for a WordPress post tagged "category-test". | String  | (empty string) |
| `numberOfPosts`      | The number of posts to be displayed.                                                                                                                                           | Integer | 1              |

## (Optional) Support for multiple WordPress installations

Starting with version 1.6.0 of this app, blog content from multiple WordPress installations is supported. To accomplish this, a `customdomainslug` parameter must be added to your store-theme's blog routes, and your WordPress blocks must be updated with various props. These steps are not required if you only wish to display blog content from a single WordPress domain.

### Store-theme example

`store/routes.json`:

```json
"store.blog-home": {
	"path": "/blog"
},
"store.blog-category": {
	"path": "/:customdomainslug/category/:categoryslug"
},
"store.blog-post": {
	"path": "/:customdomainslug/post/:slug"
},
"store.blog-search-result": {
	"path": "/:customdomainslug/search/:term"
},
"store.blog-home#page": {
	"path": "/:customdomainslug/page/:slug"
}
```

Note that the blog homepage should continue to use a specific path. It can be `blog` as in the example, or a different string of your choice. All other routes should use the `:customdomainslug` dynamic parameter.

**Blocks that use the URL params to load blog data** will need to be given a prop called `customDomains`. These blocks are:

`blog-category-list.wordpress-category-list`
`blog-post-details.wordpress-post-details`
`blog-post-details.wordpress-page-details`
`blog-search-list.wordpress-search-list`
`blog-breadcrumb.wordpress-breadcrumb`

The value of the `customDomains` prop should be a JSON list of domain "slugs" and the domain that each slug represents. The first entry should be your "default" slug and domain. For example, if you wanted URLs with the slug `blog` to load content from `http://www.blog.com/` and URLs with the slug `other-blog` to load content from `http://www.otherblog.com/`, you would give each block a prop like this:

```json
{
  "props": {
    "customDomains": "{ \"blog\":\"http://www.blog.com/\",\"other-blog\":\"http://www.otherblog.com/\" }"
  }
}
```

Make sure to follow the format of the example value, including the brackets and escaped double quotes.

**Blocks that do not use URL params** should be given a different set of props. These blocks are:

`blog-all-posts.wordpress-all-posts`
`blog-latest-posts-preview.wordpress-latest-posts-preview`
`blog-category-preview.wordpress-category-preview`
`blog-search.wordpress-search`
`search-blog-articles-preview.wordpress`
`search-blog-articles-list.wordpress`
`blog-related-posts.wordpress-related-posts`
`blog-search-list.wordpress-category-related-posts`

These blocks will accept two props: `customDomainSlug` and `customDomain`.

`customDomainSlug` must be provided for all blocks when using multiple WordPress installations. `customDomain` only needs to be provided for blocks that are **not** using the "default" WordPress domain from the app settings.

Continuing the example from above, any block that shows content from the "default" WordPress domain should receive a `customDomainSlug` prop with a value of `blog`. Any block that shows content from the secondary WordPress domain should receive a `customDomainSlug` prop with a value of `other-blog` and a `customDomain` prop with a value of `http://www.other-blog.com/`.

Example:

```json
"store.blog-home": {
    "blocks": [
      "blog-search.wordpress-search",
      "blog-latest-posts-preview.wordpress-latest-posts-preview",
      "blog-category-preview.wordpress-category-preview#test",
      "blog-all-posts.wordpress-all-posts"
    ]
  },
  "blog-search.wordpress-search": {
    "props": {
      "customDomainSlug": "blog"
    }
  },
  "blog-latest-posts-preview.wordpress-latest-posts-preview": {
    "props": {
      "title": "Latest Posts",
      "useTextOverlays": true,
      "customDomainSlug": "blog"
    }
  },
  "blog-category-preview.wordpress-category-preview#test": {
    "props": {
      "category": 5,
      "description": "Description of category",
      "useTextOverlays": false,
      "customDomain": "http://www.otherblog.com/",
      "customDomainSlug": "other-blog"
    }
  },
  "blog-all-posts.wordpress-all-posts": {
    "props": {
      "customDomainSlug": "blog"
    }
  },
```

## CSS Customization

In order to apply CSS customizations in this and other blocks, follow the instructions given in the recipe on [Using CSS Handles for store customization](https://vtex.io/docs/recipes/style/using-css-handles-for-store-customization).

| CSS Handles                            |
| -------------------------------------- |
| `breadcrumbContainer`                  |
| `breadcrumbHomeLink`                   |
| `breadcrumbLink`                       |
| `breadcrumbSeparator`                  |
| `breadcrumbCurrentPage`                |
| `categoryBlockContainer`               |
| `categoryBlockTitle`                   |
| `categoryBlockDescription`             |
| `categoryBlockFlex`                    |
| `categoryBlockFlexItem`                |
| `categoryBlockLink`                    |
| `latestPostsBlockContainer`            |
| `latestPostsBlockTitle`                |
| `latestPostsBlockFlex`                 |
| `latestPostsBlockFlexFirstColumnItem`  |
| `latestPostsBlockFlexSecondColumn`     |
| `latestPostsBlockFlexSecondColumnItem` |
| `latestPostsBlockFlexItem`             |
| `listTitle`                            |
| `listContainer`                        |
| `listFlex`                             |
| `listFlexItem`                         |
| `postFlex`                             |
| `postContainer`                        |
| `postTitle`                            |
| `postMeta`                             |
| `postFeaturedImage`                    |
| `postBody`                             |
| `postChildrenContainer`                |
| `relatedPostsBlockContainer`           |
| `relatedPostsBlockTitle`               |
| `relatedPostsBlockFlex`                |
| `relatedPostsBlockFlexItem`            |
| `searchBlockContainer`                 |
| `searchListTitle`                      |
| `searchListContainer`                  |
| `searchListFlex`                       |
| `searchListFlexItem`                   |
| `searchResultBlockContainer`           |
| `searchResultBlockTitle`               |
| `searchResultBlockFlex`                |
| `searchResultBlockFlexItem`            |
| `searchResultBlockLink`                |
| `teaserContainer`                      |
| `teaserImage`                          |
| `teaserTextOverlay`                    |
| `teaserTextOverlayTitle`               |
| `teaserTextOverlayMeta`                |
| `teaserGradientOverlay`                |
| `teaserTitle`                          |
| `teaserTitleLink`                      |
| `wordpressRelatedProducts`             |

## Troubleshooting

You can check if others are passing through similar issues [here](https://github.com/vtex-apps/wordpress-integration/issues). Also feel free to [open issues](https://github.com/vtex-apps/wordpress-integration/issues/new) or contribute with pull requests.

**Upcoming documentation:**

- [add interface](https://github.com/vtex-apps/wordpress-integration/pull/15)

## Contributing

Check it out [how to contribute](https://github.com/vtex-apps/awesome-io#contributing) with this project.

## Contributors ✨

Thanks goes to these wonderful people ([emoji key](https://allcontributors.org/docs/en/emoji-key)):

<!-- ALL-CONTRIBUTORS-LIST:START - Do not remove or modify this section -->
<!-- prettier-ignore-start -->
<!-- markdownlint-disable -->
<!-- markdownlint-enable -->
<!-- prettier-ignore-end -->

<!-- ALL-CONTRIBUTORS-LIST:END -->

This project follows the [all-contributors](https://github.com/all-contributors/all-contributors) specification. Contributions of any kind welcome!
