---
layout: post
title: Spring.NET with NHibernate and multiple session factories
comments: true
categories: [Spring.NET, NHibernate, development]
---

Why would you need multiple session factories, you ask? Well, you'll usually need it when you have mutiple users that are connecting to one or more databases with the same database schema. When you're using Spring.NET with NHibernate you have some classes available that simplyfy using NHibernate, but none of them support using multiple session factories. Unless you add it of course.

At first thought there are two possible ways to do this.
- You can extend Spring's HibernateTemplate and HibernateTransactionManager and override the SessionFactory property with one that is context sensitive.
- You can extend the LocalSessionFactoryObject and override the NewSessionFactory method so that it returns a context sensitive SessionFactory.

Of the two the second one is the best option and is also mentioned in the [documentation](http://www.springframework.net/docs/1.3.0/reference/html/orm.html). Although spring has DbProviders available for context sensitive [user credentials](http://www.springframework.net/docs/1.3.0/reference/html/dbprovider.html#dbprovider-usercredentials) and [multiple DbProviders](http://www.springframework.net/docs/1.3.0/reference/html/dbprovider.html#dbprovider-multidelegating), it doesn't have a standard LocalSessionFactoryObject that creates context sensitive SessionFactories. If you're using NHibernate 2.1 you can just follow [this thread](http://forum.springframework.net/showthread.php?t=4462) using the updated code posted by tvering.

f you're using an older version of NHibernate you'll have to set the DbProvider of the SessionFactory. Which isn't a problem unless you have to cast the SessionFactory.ConnectionProvider to an internal LocalSessionFactoryObject.DbProviderWrapper. If you read the thread until its end you'll notice that Joe Meier has added the code to the Spring.Data.NHibernate project to circumvent the problem. Which isn't such a great solution. A better one is to use reflection shown in the next few lines of code.

{% highlight C# %}
var dbProvider = (Spring.Data.Common.IDbProvider)Spring.Context.Support.ContextRegistry.GetContext().GetObject("DbProvider");
var connectionProviderType = sessionFactory.ConnectionProvider.GetType();
var connectionProviderProperty = connectionProviderType.GetProperty("DbProvider");
connectionProviderProperty.SetValue(sessionFactory.ConnectionProvider, dbProvider, null);
{% endhighlight %}

I also added a SimpleDelegatingDbProvider class if you just want to contextually set the connection string to use with the SimpleDelegatingSessionFactory.

n case you really don't like reflection and want to implement the first option, then you're out of luck. Although overriding HibernateTemplates SessionFactory property wasn't a problem, extending HibernateTransactionManager was. The needed properties and methods in the class weren't virtual so you couldn't override them. I had to copy its code into a custom transaction manager class and adjust it to my needs. But it was all in vain. At the end it didn't work as I got an exception while performing a query in the created session. It had something to do with the transaction, but I don't exactly remember what it was.
Anyway, I didn't have time to look into it any further, but if you want to try it out and want to make something contextual in Spring.NET ... the next lines of code will show you how.

{% highlight C# %}
// Set the data that you want and associate it with a name
Spring.Threading.LogicalThreadContext.SetData("associatedName" , value)
// Get the data you've set
Spring.Threading.LogicalThreadContext.GetData("associatedName").
{% endhighlight %}

Code for the second option:
[SpringNHibernateMultipleSessionFactories.zip (3.78 kb)](http://ronaldverhaegen.com/blog/file.axd?file=2010%2f6%2fSpringNHibernateMultipleSessionFactories.zip)
