+++

date = "2016-07-22"

title = "About lunacy posts"

description = "This subtitle is fixed through front matter, heh."

+++

### Front Matter

The posts' front matter holds only three variables: the date, the tile, and the description. The description is basically the subtitle you see in the main/list page of the theme. In this case, I made it obvious by writing "This subtitle is fixed through front matter, heh."

### Post iterator/pagination

Pagination is 100% operative, even though you can't see it in this demo. It's set to show 5 posts per page, and create navigation buttons below that. Nothing fancy, buy it's one less thing you have to care about. It's actually dead easy to setup, you can just change the following line in the html:

`{{ range (.Paginator 5).Pages }}`

to change the posts shown per page.

### Release the ~~kraken~~ Markdown

The beauty of Hugo is it's ease of configuration and the usage of markdown to quickly elaborate fairly rich content, capable of inserting lists, images , links, etc. to your blog/web of choice.

I encourage you to try Hugo out, explore a bit and perhaps use my theme or create your own, as it's **really** easy to get up and running!

Happy hacking! 