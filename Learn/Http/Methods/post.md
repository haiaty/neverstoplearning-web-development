According to the HTTP specification, you should use the POST method when you're using the form to change the state of something on the server end. For example, if a page has a form to allow users to add their own comments, the form should use POST. If you click "Reload" or "Refresh" on a page that you reached through a POST, it's almost always an error -- you shouldn't be posting the same comment twice -- which is why these pages aren't bookmarked or cached.

POST verb usually means creation of new resource
