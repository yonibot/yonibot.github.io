---
layout: post
title: "find_by versus find in Rails"
date: 2013-06-10 15:13
comments: true
categories: [Rails, DB, SQL]
---
_Edit: This article is a bit dated, since Rails 3 prefers the **where** method to the **find **method._

Rails has a number of distinct ways to find ActiveRecord objects by id. They are:

Object**.find(id)**

Object**.find_by_id(id)**

Object**.find_all_by_id(id)**

How do you know which one to use? Does it matter?

I found that all 3 methods have their own unique properties, and can be used when you're looking for specific types of results. The differences can be expressed as follows:

1. **Returning an ActiveRecord error ****or nil - **The basic 'find' method throws an ActiveRecord::RecordNotFound error if you input an id that does not correspond to object. The other two methods return nil.

2. **Whether multiple objects can be returned** - The 'find_by' syntax only returns a single object. In contrast, the 'find' and 'find_all_by_' can return multiple objects. If you input multiple id's into a find_by method, it will simply return the first result.**
**

3. **Whether objects are always returned as an array -** For the 'find_all_by' method, the value is always returned in an array - even if the result is a single object.

In summary:
<table style="background-color:#ffffcc;" width="100%" border="1" cellspacing="1" cellpadding="3">
<tbody>
<tr>
<td><span style="text-decoration:underline;">Method</span> / <span style="text-decoration:underline;">Result</span></td>
<td style="text-align:center;">**Error or Nil?**</td>
<td style="text-align:center;">**Number of Objects it can Return**</td>
<td style="text-align:center;">**Always returns an array?**</td>
</tr>
<tr>
<td>**.find**</td>
<td>ActiveRecord error</td>
<td>Multiple</td>
<td>No</td>
</tr>
<tr>
<td>**.find_by**_</td>
<td>nil</td>
<td>Single</td>
<td>No</td>
</tr>
<tr>
<td>**.find_all_by_**</td>
<td>nil</td>
<td>Multiple</td>
<td>Yes, even for a single object.</td>
</tr>
</tbody>
</table>

[ ](http://www.quackit.com/html/html_table_tutorial.cfm)