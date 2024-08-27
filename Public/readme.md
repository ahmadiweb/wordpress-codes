# General code

> In one line of the title

```
.class {
    display: block;
    display: -webkit-box;
    max-width: 250px; // Your max-width of title
    -webkit-line-clamp: 1; // Number of lines
    -webkit-box-orient: vertical;
    overflow: hidden;
    text-overflow: ellipsis;
}
```

> Close the WordPress API to the public

```
function disable_api_function()
{
    if (current_user_can('administrator') || current_user_can('editor')) { // You can add any level of your website
        return;
    } else {
        add_filter('rest_authentication_errors', 'disable_rest_api');
        function disable_rest_api($access)
        {
            return new WP_Error(
                'rest_disabled',
                __('The WordPress REST API has been disabled.'),
                array('status' =>
                rest_authorization_required_code())
            );
        }
    }
}
disable_api_function();
```