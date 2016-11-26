``` javascript
/**
 * Show specific bookmark page of questions
 *
 * @param base_id
 * @param bookmark_id
 * @param page
 * @param callback
 */
function showBookmarkQuestionPage(base_id, bookmark_id, page, callback) {
    $.post('/bookmark/' + bookmark_id, {
        page : page,
        itemInPage : 10,
        type : 'question'
    }, function(results) {
        if (!results.data.length) {
            return ;
        }
        var template = Handlebars.templates['_subscribed_question.html'];
        // show question bookmark
        $('#' + base_id + '_content').html(template({
            questions : results.data
        }));
        // show navbar pagination
        $('#' + base_id + '_nav').html(
            compilePageNav(page, results.pages, base_id, 'question', bookmark_id, 'showBookmarkQuestionPage')
        );

        // check callback
        if (callback && typeof callback == "function") {
            callback(results);
        }
    })
}
```



``` html
<!--this is the template for bookmark item-->
<!--the template will rendered by javascript template engine `handlebars.js`-->
{{#each bookmarks}}
<div class="userHome_layoutItem">
    <div><strong><a href="/bookmark/{{id}}">{{name}}</a></strong></div>
    <div class="font-grey">{{numQuestion}} Questions • {{numAnswer}} Answers • {{numSubscriber}} Subscribers </div>
    {{#each representatives}}
        <div><span class="font-greyLight">{{type}}:&nbsp;</span><a href="{{url}}">{{name}}</a></div>
    {{/each}}
    <div><a href="/bookmark/{{id}}" class="font-greyLight">More&gt;&gt;</a></div>
</div>
<hr class="small_hrLight">
{{/each}}
```