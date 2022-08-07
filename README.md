# javascript-tree-menu (nodejs using ejs)
tree menu from row data to tree in html using tag &lt;ul> &lt;li> &lt;/li> &lt;/ul>

1. Data from database:
```
[
    {"id": 123, "parentid": 0, "name": "Root"},
    {"id": 456, "parentid": 123, "name": "Child 456"},
    {"id": 214, "parentid": 456, "name": "Child 214"},
    {"id": 810, "parentid": 456, "name": "Child 810"},
    {"id": 919, "parentid": 456, "name": "Child 919"}
]
```

2. Program:
```
const arr = [
    {"id": 123, "parentid": 0, "name": "Root"},
    {"id": 456, "parentid": 123, "name": "Child 456"},
    {"id": 214, "parentid": 456, "name": "Child 214"},
    {"id": 810, "parentid": 456, "name": "Child 810"},
    {"id": 919, "parentid": 456, "name": "Child 919"}
];
const {res} = arr.reduce((acc,curr)=>{
  if(acc.parentMap[curr.parentid]){
    (acc.parentMap[curr.parentid].children = 
           acc.parentMap[curr.parentid].children || []).push(curr);
  } else {
    acc.res.push(curr);
  }
  acc.parentMap[curr.id] = curr;
  return acc;
}, {parentMap: {}, res: []});
console.log(JSON.stringify(res));
```

3. Json output:
```
[{"id":123,"parentid":0,"name":"Root","children":[{"id":456,"parentid":123,"name":"Child 456","children":[{"id":214,"parentid":456,"name":"Child 214"},{"id":810,"parentid":456,"name":"Child 810"},{"id":919,"parentid":456,"name":"Child 919"}]}]}]
```

4. main.ejs:
```
<% tree = [{"id":123,"parentid":0,"name":"Root","children":[{"id":456,"parentid":123,"name":"Child 456","children":[{"id":214,"parentid":456,"name":"Child 214"},{"id":810,"parentid":456,"name":"Child 810"},{"id":919,"parentid":456,"name":"Child 919"}]}]}] %>
<%- partial('treeView', {items: tree}) %>
```

5. treeView.ejs:
```
<% if (items.length) { %>
<ul>
<% } %>

    <% items.forEach(function(item){ %>
    <li>
        <a href="<%= item.link %>"><%= item.name %></a>
        <%- include('treeView', {items: item.children}) %>
    </li>
    <% }) %>

<% if (items.length) { %>
</ul>
<% } %>
```

6. output:
