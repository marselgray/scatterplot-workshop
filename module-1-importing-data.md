## Importing And Rendering Data 

The starter code for this section has been provided so please fork the following codepen: 

**Starter Code:** [D3 - Scatterplot - Importing Data - Starter](https://codepen.io/jkeohan/pen/RQPEox)

D3's main focus is rendering charts and graphs that represent one or more datasets. Sometimes the data can be generated on the spot, like using an array of numbers to render circles, but most of the time D3 first needs to import,  parser, and format data before it can render it in the form and shape dictated by the visualization.  D3 comes with several [convenience methods](https://github.com/d3/d3-fetch) that either use callbacks or that run on top of the Fetch API and support importing the following data types:

- d3.text, d3.csv, d3.tsv, d3.dsv, d3.json, d3.image

There are quite a few more, but these methods would be able to import the most commonly used formats of datasets. 

#### Using D3.CSV

For the purpose of the scatterplot we will initially test the d3.csv() method, included in the D3V4 library, to import the data and then convert it to be used with the newly introduced d3-fetch module. 

```
d3.csv(url, (d) =>  { console.log('data is: ', d)})

// => data is:  (42) [{…}, {…}, {…}, {…},
```

#### Using D3-FETCH

**Note:** d3.request has been replaced with d3.fetch in D3 5.0

Although the basic d3.csv() method is sufficient to import the csv data, D3 also comes with additional modules such as [d3-fetch](https://github.com/d3/d3-fetch/blob/master/README.md#csv) which runs on top of [fetch API](https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API).  Once the module is imported it extends the functionality of the previous convenience methods by providing .then().  

Open the JS Settings in the CodePen and add the following url:

```
https://d3js.org/d3-fetch.v1.min.js
```

With the module loaded, lets refactor our previous example to work with d3-fetch.

```
d3.csv(url).then((d) => console.log('data is: ', d)) 

// => data is:  (42) [{…}, {…}, {…}, {…}, {…}
```

We can see that it returns the dataset as an array of objects which are formats that lend themselves quite nicely for organizing data.   The method also comes with a callback in .csv() which allows for data manipulation before being passed to .then().  Let's add the callback and see what it returns:

```
d3.csv(url,(d) => { console.log('first cb', d); return d})
	.then((d) => console.log('second cb', d)) 

// => first cb {2002: "12.7", 2003: "12.6", 2004: "12.4", 2005: "12.4",
// => second cb (42) [{…}, {…}, {…}, {…}, {…}
```

Importing data is one of the very first steps in building out a visualization so play with the additional formats.  With that being said, let's take a look at one more method used for importing data that makes it easier to work with multiple asynchronous functions that use callbacks.  Enter...[d3-queue](https://github.com/d3/d3-queue).


#### Using D3-QUEUE: Instructor Demo Only

**Note:** d3-queue is deprecated in D3v5

The instructor will demo this d3-queue module but will not use it in this project.  It's only meant to demonstrate the functionality. 

This d3-queue module is also a separate library and so the JS setting of the codepen must be edited once again. 


```
https://d3js.org/d3-queue.v3.min.js
```

With the module imported we can use d3.queue() to import data.

```
d3.queue()
 .defer(d3.csv, url)
 .await(function (err, data) {
  console.log('data is: ', data )
});

// => data is:  [{…}, {…}, {…}, {…}, {…}
  
```

### Using LocalStorage 

Since we're going to be making edits consistently throughout this project, we want to limit the calls being made to github to retrieve the data.  Everytime an edit is made the codepen executes, and every line of code that is called will rerun.  One simple way to limit that, is to make the call successfully once and store the data in localStorage.  This lecture isn't meant to go into the details of localStorage so the following **getData** function has been provided. Delete all the previous code, and copy and paste the below into the codepen.

```
getData()
function getData() {
  let url = "https://gist.githubusercontent.com/jkeohan/974c071a5d4d0185a846/raw/971a9b8dfc0ebe238ee271611991cd98e6cac434/data_regions.csv";
  let data = localStorage.getItem("countries");
  if (data) {
    render(data);
  } else {
    d3.csv(url).then((data) => {
      localStorage.setItem("countries", JSON.stringify(data));
       render(data);
    }) 
  }
}
```
As we review the function we notice there is a render function being called, however, it has not yet been created so let's do that.  Above the getData() function call, let's create a new **render** function and console.log the data. 

```
function render(data) {
  console.log('this is render ', data)
}

getData()
function getData(data) {...}
```

#### Solution Code

Here is the full solution code for the project thus far:

[D3 - Scatterplot - Importing Data - Solution ](https://codepen.io/jkeohan/pen/XZGjVV?editors=0010)

#### Additional Resources

- [Changes to D3v5](https://github.com/d3/d3/blob/master/CHANGES.md#changes-in-d3-50)
- [d3-fetch]()
- [d3-queue]()
- [d3-queue-mental-model](https://macwright.org/2016/12/09/a-d3-queue-mental-model.html)
