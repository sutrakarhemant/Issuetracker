// # Issuetracker
// issue tracker are create issue &amp;  create and delete
const express = require('express');
const bodyParser = require('body-parser');

const app = express();
app.use(bodyParser.urlencoded({ extended: false }));

// In-memory array to store issues
const issues = [];

app.get('/', (req, res) => {
  let issueList = '';
  for (let i = 0; i < issues.length; i++) {
    issueList += `
      <li>
        ${issues[i].title}
        <form action="/delete_issue" method="POST" style="display:inline;">
          <input type="hidden" name="index" value="${i}">
          <button type="submit">Delete</button>
        </form>
      </li>
    `;
  }

  const html = `
    <h1>Issue Tracker</h1>
    <a href="/create_issue">Create Issue</a>
    <ul>
      ${issueList}
    </ul>
  `;
  res.send(html);
});

app.get('/create_issue', (req, res) => {
  const html = `
    <h1>Create Issue</h1>
    <form action="/create_issue" method="POST">
      <label for="title">Title:</label>
      <input type="text" id="title" name="title" required><br><br>
      <label for="description">Description:</label>
      <textarea id="description" name="description" required></textarea><br><br>
      <input type="submit" value="Create">
    </form>
  `;
  res.send(html);
});

app.post('/create_issue', (req, res) => {
  const title = req.body.title;
  const description = req.body.description;
  // Create a new issue
  const issue = { title, description };
  issues.push(issue);
  res.redirect('/');
});

app.post('/delete_issue', (req, res) => {
  const index = req.body.index;
  if (index >= 0 && index < issues.length) {
    issues.splice(index, 1);
  }
  res.redirect('/');
});

app.listen(3000, () => {
  console.log('Issue tracker app is listening on port 3000!');
});
