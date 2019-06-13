# [Introduction to HTML](https://www.codecademy.com/learn/learn-html)

## 1 Elements and Structure

**HTML** stands for **H**yper**T**ext **M**arkup **L**anguage and is used to create the structure and content of a webpage.

### 1.1 `<h1>` to `<h6>`

Headings and sub-headings, `<h1>` to `<h6>` tags, are used to enlarge text.

### 1.2 `<p>`, `<span>` and `<div>`

`<p>`, `<span>` and `<div>` tags specify text or blocks.

### 1.3 `<em>` and `<strong>`

The `<em>` and `<strong>` tags are used to emphasize text.

- The `<em>` tag will generally render as *italic* emphasis. (emphasizes text)
- The `<strong>` tag will generally render as **bold** emphasis. (highlights important text)

### 1.4 `<br>`

Line breaks are created with the `<br>` tag.

The line break element is unique because it is *only* composed of a starting tag. 

### 1.5 `<ul>`, `<ol>` and `<li>`

Ordered lists (`<ol>`) are numbered and unordered lists (`<ul>`) are bulleted.

### 1.6 `<img>` and `<video>`

#### 1.6.1 Image Alts

```html
<img src="#" alt="A field of yellow sunflowers" />
```

The `alt` attribute also serves the following purposes:

- If an image fails to load on a web page, a user can mouse over the area originally intended for the image and read a brief description of the image. This is made possible by the description you provide in the alt attribute.
- Visually impaired users often browse the web with the aid of screen reading software. When you include the alt attribute, the screen reading software can read the image’s description out loud to the visually impaired user.
- The alt attribute also plays a role in Search Engine Optimization (SEO), because search engines cannot “see” the images on websites as they crawl the internet. Having descriptive alt attributes can improve the ranking of your site.

#### 1.6.2 `<video>`

- After the src attribute, the width and height attributes are used to set the size of the video displayed in the browser. 
- The **controls** attribute instructs the browser to include basic video controls: pause, play and skip.(Otherwise the video cannot be played, showing the first frame that is still)
- The text, “Video not supported”, between the opening and closing video tags will only be displayed if the browser is unable to load the video.

```html
<video src="myVideo.mp4" width="320" height="240" controls>
Video not supported
</video>
```

### 1.7 review

```html
<body>
  <h1>The Brown Bear</h1>
  <div id="introduction">
    <h2>About Brown Bears</h2>
    <p>The brown bear (<em>Ursus arctos</em>) is native to parts of northern Eurasia and North America. Its conservation status is currently <strong>Least Concern</strong>.<br /><br /> There are many subspecies within the brown bear species, including the Atlas bear and the Himalayan brown bear.</p>
    <h3>Species</h3>
    <ul>
      <li>Arctos</li>
      <li>Collarus</li>
      <li>Horribilis</li>
      <li>Nelsoni (extinct)</li>
    </ul>
    <h3>Features</h3>
    <p>Brown bears are not always completely brown. Some can be reddish or yellowish. They have very large, curved claws and huge paws. Male brown bears are often 30% larger than female brown bears. They can range from 5 feet to 9 feet from head to toe.</p>
  </div>
  <div id="habitat">
    <h2>Habitat</h2>
    <h3>Countries with Large Brown Bear Populations</h3>
    <ol>
      <li>Russia</li>
      <li>United States</li>
      <li>Canada</li>
    </ol>
    <h3>Countries with Small Brown Bear Populations</h3>
    <p>Some countries with smaller brown bear populations include Armenia, Belarus, Bulgaria, China, Finland, France, Greece, India, Japan, Nepal, Poland, Romania, Slovenia, Turkmenistan, and Uzbekistan.</p>
  </div>
  <div id="media">
    <h2>Media</h2>
    <img src="https://s3.amazonaws.com/codecademy-content/courses/web-101/web101-image_brownbear.jpg" alt="A Brown Bear"/>
    <video src="https://s3.amazonaws.com/codecademy-content/courses/freelance-1/unit-1/lesson-2/htmlcss1-vid_brown-bear.mp4" width="320" height="240" controls>
      Video not supported
    </video>
  </div>
</body>
```

## 2 HTML Document Standards

### 2.1 `<!DOCTYPE html>`

The `<!DOCTYPE html>` declaration should always be the first line of code in your HTML files. This lets the browser know what version of HTML to expect.

### 2.2 `<html>`

The `<html>` element will contain all of your HTML code.

### 2.3 `<head>` and `<title>`

Information about the web page, like the title, belongs within the `<head>` of the page.

You can add a title to your web page by using the `<title>` element, inside of the head.A webpage’s title appears in a browser’s tab.

### 2.4 `<a>`

Anchor tags (`<a>`) are used to link to internal pages, external pages or content on the same page.

```html
<nav>
    <a href="./index.html" target="_blank">Brown Bear</a>
    <a href="./aboutme.html">About Me</a>
</nav>
```

You can create sections on a webpage and jump to them using `<a>` tags and adding `id`s to the elements you wish to jump to.

```html
<ul>
    <li><a href="#introduction">Introduction</a></li>
    <li><a href="#habitat">Habitat</a></li>
    <li><a href="#media">Media</a></li>
</ul>
```
### 2.5 `<!-- comment -->`

Comments are written in HTML using the following syntax: `<!-- comment -->`.

## 3 Tables

### 3.1 `<table>`

The `<table>` element creates a table.

### 3.2 `<tr>`

The `<tr>` element adds rows to a table.

### 3.3 `<td>` and '<th>'

To add data to a row, you can use the `<td>` element.

able headings clarify the meaning of data. Headings are added with the `<th>` element.

### 3.4 `colspan` and `rowspan`

Table data can span columns using the colspan attribute.

Table data can span rows using the rowspan attribute.

### 3.6 `<thead>`, `<tbody>` and `<tfoot>`

Tables can be split into three main sections: a head, a body, and a footer.

### 3.7 review

```html
  <table>
    <thead>     <!-- thead -->
      <tr>
        <th>Company Name</th>
        <th>Number of Items to Ship</th>
        <th>Next Action</th>
      </tr>
    </thead>
    <tbody>     <!-- tbody -->
      <tr>
        <td>Adams Greenworks</td>
        <td>14</td>
        <td>Package Items</td>
      </tr>
    </tbody>
    <tfoot>     <!-- tfoot -->
      <td>Total</td>
      <td>28</td>
    </tfoot>
  </table>
```

## 4 Forms

### 4.1 `<form>`

The purpose of a `<form>` is to allow users to input information and send it.

Two attributes:
- `action` determines where the form’s information goes.
- `method` determines how the information is sent and processed.

```html
<form action="submission.html" method="POST">
</form>
```

### 4.2 `input`

four basic attributes:
- type
- name
- value
- id

**form return name=value**

To add fields for users to input information we use the `input` element and *set the type* attribute to a field of our choosing:

#### 4.2.1 `<input type="text">`

Setting type to "text" creates a single row field for text input.

#### 4.2.2 `<input type="password">`

Setting type to "password" creates a single row field that censors text input.

#### 4.2.3 `<input type="number">`

Setting type to "number" creates a single row field for number input.

#### 4.2.4 `<input type="range">`

Setting type to "range" creates a slider to select from a range of numbers.

```html
<input type="range" name="doneness" id="doneness" value="3" min="1" max="5">
```

#### 4.2.5 `<input type="checkbox">`

Setting type to "checkbox" creates a single checkbox which can be paired with other checkboxes.

```html
<section class="toppings">
    <span>What toppings would you like?</span>
    <br>
    <input type="checkbox" name="topping" id="lettuce" value="lettuce">
    <label for="lettuce">Lettuce</label>
    <input type="checkbox" name="topping" id="tomato" value="tomato">
    <label for="tomato">Tomato</label>
    <input type="checkbox" name="topping" id="onion" value="onion">
    <label for="onion">Onion</label>
</section>
```

#### 4.2.6 `<input type="radio">`

Setting type to "radio" creates a radio button that can be paired with other radio buttons.

```html
<section class="cheesy">
    <span>Would you like to add cheese?</span>
    <br>
    <input type="radio" name="cheese" id="yes" value="yes">  <!-- name -->
    <label for="yes">Yes</label>
    <input type="radio" name="cheese" id="no" value="yes">
    <label for="no">No</label>
</section>
```

#### 4.2.7 Setting type to "list"

Setting type to "list" will pair the `<input>` with a `<datalist>` element.

#### 4.2.8 `<input type="submit">`

Setting type to "submit" creates a submit button.

### 4.3 `<select>`

A `<select>` element is populated with `<option>` elements and renders a dropdown list selection.

```html
<section class="bun-type">
    <label for="bun">What type of bun would you like?</label>
    <select name="bun" id="bun">
        <option value="sesame">Sesame</option>
        <option value="potatoe">Potato</option>
        <option value="pretzel">Pretzel</option>
    </select>
</section>
```

### 4.4 `<datalist>`

A `<datalist>` element is populated with `<option>` elements and works with an `<input>` to search through choices.

```html
<section class="sauce-selection">
    <label for="sauce">What type of sauce would you like?</label>
    <input list="sauces" id="sauce" name="sauce"> <!-- list="sauces" => id -->

    <datalist id="sauces">
        <option value="ketchup"></option>
        <option value="mayo"></option>
        <option value="mustard"></option>
    </datalist>
</section>
```

### 4.5 reviews

```html
<html lang="en" dir="ltr">
  <head>
    <meta charset="utf-8">
    <link rel="stylesheet" type="text/css" href="style.css">
    <link href="https://fonts.googleapis.com/css?family=Rubik" rel="stylesheet">
    <title>Forms Review</title>
  </head>
  <body>
    <section id="overlay">
      <img src="https://s3.amazonaws.com/codecademy-content/courses/web-101/unit-6/htmlcss1-img_burger-logo.svg" alt="Davie's Burgers Logo" id="logo">
      <hr>
      <form action="submission.html" method="POST">
				<h1>Create a burger!</h1>
        <section class="protein">
          <label for="patty">What type of protein would you like?</label>
    			<input type="text" name="patty" id="patty">
        </section>
        <hr>
        
        <section class="patties">
          <label for="amount">How many patties would you like?</label>
          <input type="number" name="amount" id="amount">
        </section>
        <hr>
        
        <section class="cooked">
          <label for="doneness">How do you want your patty cooked</label>
          <br>
          <span>Rare</span>
          <input type="range" name="doneness" id="doneness" value="3" min="1" max="5">
          <span>Well-Done</span>
        </section>
        <hr>
        
        <section class="toppings">
          <span>What toppings would you like?</span>
          <br>
          <input type="checkbox" name="topping" id="lettuce" value="lettuce">
          <label for="lettuce">Lettuce</label>
          <input type="checkbox" name="topping" id="tomato" value="tomato">
          <label for="tomato">Tomato</label>
          <input type="checkbox" name="topping" id="onion" value="onion">
          <label for="onion">Onion</label>
        </section>
        <hr>
        
        <section class="cheesy">
          <span>Would you like to add cheese?</span>
          <br>
          <input type="radio" name="cheese" id="yes" value="yes">
          <label for="yes">Yes</label>
          <input type="radio" name="cheese" id="no" value="yes">
          <label for="no">No</label>
        </section>
        <hr>
       
        <section class="bun-type">
          <label for="bun">What type of bun would you like?</label>
          <select name="bun" id="bun">
            <option value="sesame">Sesame</option>
            <option value="potatoe">Potato</option>
            <option value="pretzel">Pretzel</option>
          </select>
        </section>
        <hr>
        
        <section class="sauce-selection">
          <label for="sauce">What type of sauce would you like?</label>
          <input list="sauces" id="sauce" name="sauce">
          <datalist id="sauces">
            <option value="ketchup"></option>
            <option value="mayo"></option>
            <option value="mustard"></option>
          </datalist>
        </section>
        <hr>
        <section class="extra-info">
          <label for="extra">Anything else you want to add?</label>
          <br>
          <textarea id="extra" name="extra" rows="3" cols="40"></textarea>
        </section>
        <hr>

        <section class="submission">
          <input type="submit" value="Submit">
        </section>
      </form>
    </section>
  </body>
</html>
```

## 5 Form Validation

### 5.1 `required` attribute

Adding the `required` attribute to an input related element will validate that the input field has information in it.

```html
<input id="allergies" name="allergies" type="text" required>
```

### 5.2 `min` and `max` attribute

Assigning a value to the `min` attribute of a number input element will validate an acceptable minimum value.

```html
<label for="guests">Enter # of guests:</label>
<input id="guests" name="guests" type="number" min="1" max="4">     <!-- type="number" -->
```


### 5.3 `minlength` and `maxlength` attribute

Assigning a value to the `minlength` attribute of a text input element will validate an acceptable minimum number of characters.

```html
<label for="pw">Password:</label>
<input id="pw" name="pw" type="password" minlength="8" maxlength="15" required>
```

### 5.4 `pattern` attribute

Assigning a regex to `pattern` matches the input to the provided regex.

```html
<label for="payment">Credit Card Number (no spaces):</label>
<input id="payment" name="payment" type="text" required pattern="[0-9]{14,16}">
```

### 5.5 review

```html
<!DOCTYPE html>
<html lang="en" dir="ltr">
  <head>
    <meta charset="utf-8">
    <title>Sign Up Page</title>
    <link rel="stylesheet" href="style.css" type="text/css">
    <link href="https://fonts.googleapis.com/css?family=Fjalla+One" rel="stylesheet">
  </head>
  <body>
    <section class="overlay">
			<h1>Sign Up</h1>
      <p>Create an account:</p>
      <form action="submission.html" method="GET">
        <label for="username">Username:</label>
        <br>
				<input id="username" name="username" type="text" required minlength="3" maxlength="15">
        <br>
        <label for="pw">Password:</label>
        <br>
        <!--Add the pattern attribute to the input below-->
				<input id="pw" name="pw" type="password" required minlength="8" maxlength="15">
        <br>
        <input type="submit" value="Submit">
      </form>
    </section>
  </body>
</html>
```

