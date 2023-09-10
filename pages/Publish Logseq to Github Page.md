- First for all, [enable github pages on your repository](https://docs.github.com/en/pages/quickstart).
- Use [Logseq official sulotion](https://github.com/logseq/publish-spa) for publishing:
	- Add the following content to `.github/workflows/publish.yml`
		- ```yml
		  on: [push]
		  
		  permissions:
		    contents: write
		  jobs:
		    test:
		      runs-on: ubuntu-latest
		      name: Publish Logseq graph
		      steps:
		        - uses: actions/checkout@v3
		        - uses: logseq/publish-spa@v0.2.0
		        - name: add a nojekyll file # to make sure asset paths are correctly identified
		          run: touch $GITHUB_WORKSPACE/www/.nojekyll
		        - name: Deploy 🚀
		          uses: JamesIves/github-pages-deploy-action@v4
		          with:
		            folder: www
		  ```
	- In your repo, edit `Settings -> Pages -> Build and deployment -> Branch` to `gh-pages`
- Push your local git repo for to github, and wait for github actions to complete
- After all above, you'll be able to access the published Logseq notes in your `xxxx.github.io` site
- Remeber to [configure your graph](https://docs.logseq.com/#/page/publishing/block/configuration)