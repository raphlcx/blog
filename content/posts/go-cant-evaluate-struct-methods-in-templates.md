---
title: "Go: Can't evaluate struct methods in templates"
date: 2022-04-04T20:47:00+08:00
---
You have a User struct and a struct method for it:

```
type User struct {
	FirstName string
	LastName string
}

func (u *User) FullName() string {
	return u.FirstName + " " + u.LastName
}
```

Now, you want to display the full name of the user in an HTML template:

```
user := User{FirstName: "foo", LastName: "bar"}

tmpl, _ := template.ParseFiles("index.html")

if err := tmpl.Execute(os.Stdout, user); err != nil {
	log.Print(err)
}
```

Within the `index.html` template file:

```
<p>My full name is {{ .FullName }}</p>
```

Executing the code gives:

```
<p>My full name is 2022/04/04 20:58:12 template: index.html:1:22: executing "index.html" at <.FullName>: can't evaluate field FullName in type main.User
```

But isn't `.FullName` a struct method on the User struct?

The solution is to use a non-pointer type on the struct method:

```
-- func (u *User) FullName() string {
++ func (u User) FullName() string {
```

Either that or you can retain the pointer type on the struct method but pass in a pointer to the struct on `template.Execute()`:

```
-- if err := tmpl.Execute(os.Stdout, user); err != nil {
++ if err := tmpl.Execute(os.Stdout, &user); err != nil {
```

Execute again. Now it works:

```
<p>My full name is abby tebby</p>
```

{{< rawhtml >}}
<details>
  <summary>Full source code (main.go)</summary>
	<pre><code>package main

import (
	"log"
	"os"
	"text/template"
)

type User struct {
	FirstName string
	LastName  string
}

func (u User) FullName() string {
	return u.FirstName + " " + u.LastName
}

func main() {
	user := User{FirstName: "abby", LastName: "tebby"}

	tmpl, _ := template.ParseFiles("index.html")

	if err := tmpl.Execute(os.Stdout, user); err != nil {
		log.Print(err)
	}

	return
}</code></pre>
</details>

<details>
  <summary>Full source code (index.html)</summary>
  <pre><code>&lt;p>My full name is {{ .FullName }}&lt;/p></code></pre>
</details>
{{< /rawhtml >}}
