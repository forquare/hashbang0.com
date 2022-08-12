# Workflow

## Developing

Developing is for anything from theme work to creating new content or modifying old content.

Create a branch called something like `dev_<branch_name>` and push, inetd in the web jail will pull and build hashbang0.com on every git push.  The resulting site can be found at https://dev.hashbang0.com/

## Testing

Once you are happy with the dev work, merge your changes into the `test` branch.  Once pushed, this will be available at https://test.hashbang0.com

## Deploying

Once happy that test works, you can merge to `main` and find the site at the regular https://hashbang0.com

## Stats page

The "stats" page isn't really about stats, it just logs what branch is built at what time.  Useful to see when the current site your looking at was published.

* https://dev.hashbang0.com/build_date.txt
* https://test.hashbang0.com/build_date.txt
* https://hashbang0.com/build_date.txt
