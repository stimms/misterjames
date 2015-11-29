title: Naming the Database Created by EF Code First
tags:
  - Asp.Net MVC
  - Entity Framework
  - MvcScaffolding
id: 1181
categories:
  - Code Dive
date: 2011-06-02 18:47:00
---

<div>If you don’t want to have a database name like:</div> <div><pre>holy.crap.this.is.a.long.name.with.my.whole.namespace.contextname</pre>…then you simply need to name the database.&nbsp; To do this, we need to create the class that inherits from DbContext and call the base constructor to pass in the name. <pre>&nbsp;&nbsp;&nbsp; public class FamilyContext : 

DbContext&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; {&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; public FamilyContext (): base("FamilyContext") {}&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; }</pre>
<div>&nbsp;</div>
<div>Sweet.&nbsp; So, one other thing to pay attention to then, especially if you’re using MvcScaffolding or other similar tools.&nbsp; If you’ve named your context you’ll need to tell it where you want your entity collections to manifest.&nbsp; For MvcScaffolding in particular, you can use the –DbContext switch and pass in your context name.&nbsp; That looks something like this:</div><pre>Scaffold Controller Family -DbContext Family</pre>
<div>The result is that MvcScaffolding will now look for that type in the target project and add the appropriate code.</div>
<div></div>
<div><pre>    public class FamilyContext : DbContext
    {
        public FamilyContext (): base("FamilyContext") {}
        public DbSet&lt;MyProject.Models.Family&gt; Families { get; set; }     
    }</pre></div>
<div></div>
<div>Hope this helps! </div></div>