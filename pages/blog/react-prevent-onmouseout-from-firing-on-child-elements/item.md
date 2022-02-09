---
title: 'React - Prevent onmouseout from firing on child elements'
media_order: simona-sergi-x0VqbsCaFlw-unsplash.jpg
date: '20:25 13-03-2021'
taxonomy:
    category:
        - React
    tag:
        - React
        - 'Mouse events'
hide_git_sync_repo_link: false
hero_classes: text-light
hero_image: simona-sergi-x0VqbsCaFlw-unsplash.jpg
blog_url: /blog
show_sidebar: true
show_breadcrumbs: true
show_pagination: true
hide_from_post_list: false
feed:
    limit: 10
---

===

Given the following:

>     render() {
>     
>       const onMouseOver = (event) => {
>         this.setState({ isHovering: true });
>       };
>     
>       const onMouseOut = (event) => {
>         this.setState({ isHovering: false });
>       };
>     
>       const open = this.state.isHovering ? true : false;
>     
>       return (
>         <Nav className={styles.TopNav} bsStyle="pills" activeKey={1}>
>           <NavDropdown
>             className={dropDownClasses}
>             eventKey={1}
>             open={open}
>             title="Cards"
>             id="nav-dropdown"
>             onMouseEnter={onMouseOver}
>             onMouseOut={onMouseOut}>
>             <MenuItem eventKey="1.1">Action</MenuItem>
>             <MenuItem eventKey="1.2">Another action</MenuItem>
>           </NavDropdown>
>           <NavItem className={styles.navLink} eventKey={2}>Blog</NavItem>
>         </Nav>
>       );

How do I prevent `onMouseOut` firing when the mouse is over a child of `NavDropdown`?

Can I detect if I am over a child in `onMouseOut`?



### Solution
This is not React specific. `mouseover` and `mouseout` events bubble, so the handler also triggers for children of the element. `mouseenter` and `mouseleave` don't bubble.

So you should listen to `mouseleave` instead.

### Source
[StackOverflow - https://stackoverflow.com/questions/34071798/prevent-onmouseout-from-firing-on-child-elements](https://stackoverflow.com/questions/34071798/prevent-onmouseout-from-firing-on-child-elements)

[Archive - https://archive.ph/fqLVu](https://archive.ph/fqLVu)