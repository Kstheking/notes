# xpath

## [xpath tutorial](https://www.w3schools.com/xml/xpath_intro.asp)
## [xpath 1.0 library functions](https://www.guru99.com/using-contains-sbiling-ancestor-to-find-element-in-selenium.html)

### multiple predicates 

`/root/user[login = 'user1' and name='User 1' and profile='admin' and profile='operator']`  
so we use and for multiple predicates 

#### it seems like you need to begin the xpath by either / or // you most probably can't directly ask for seach like `bookstore` (this happens because more than likely bookstore won't be on your root, root elements would be html, head, body etc)

#### an example using contains and following-sibling
`//label[contains(text(),'Gender')]/following-sibling::div`  
#### to select just the node after a label 
`//label[contains(text(),'Gender')]/following-sibling::node()`

#### a node with a certain descendant 
`//div[contains(@class,'my_team_member_card')][descendant::*[contains(text(),'Active Boy')]]`
