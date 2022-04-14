---
title: 'How to make React-Material UI data grid to render rows using alternate shades?'
media_order: 'eberhard-grossgasteiger-2R8Sz0dHMj0-unsplash.jpg,uiAdu.png,HlD8X.png'
taxonomy:
    category:
        - react
        - 'material react'
    tag:
        - React
hide_git_sync_repo_link: false
hero_classes: text-light
hero_image: eberhard-grossgasteiger-2R8Sz0dHMj0-unsplash.jpg
blog_url: /blog
show_sidebar: true
show_breadcrumbs: true
show_pagination: true
hide_from_post_list: false
feed:
    limit: 10
---

I am evaluating React-Material grid for my customer. One of the key feedback was the absence of alternate shading of rows, which impacts user experience.

[https://material-ui.com/components/data-grid/#mit-version](https://material-ui.com/components/data-grid/#mit-version)

Is this possible?

Actual
![HlD8X](HlD8X.png "HlD8X")

Desired
![uiAdu](uiAdu.png "uiAdu")

===

### Solution

It's pretty simple to add in via a CSS selector.

If you add a selector for the odd rows `.Mui-odd`, then you can set the color of the background and make it striped. You could use `.Mui-even` instead for the other half.

>     .MuiDataGrid-row.Mui-odd {
>       background-color: aliceblue;
>     }

If you wanted to use `:nth-child`, then you'd need `:nth-child(even)` for the same set of rows as `.Mui-odd`, though `.Mui-odd` keeps up it's ordering between pages, where the psuedo selector doesn't.

>     .MuiDataGrid-row:nth-child(even){
>       background-color: aliceblue;
>     }


### Source
    
[StackOverflow - How to make React-Material UI data grid to render rows using alternate shades?](https://stackoverflow.com/questions/64682097/how-to-make-react-material-ui-data-grid-to-render-rows-using-alternate-shades)
