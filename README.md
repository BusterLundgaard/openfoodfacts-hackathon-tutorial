# A super brief history of the web
The WORLD WIDE WEB, with its HTTP and HTML and JAVSCRIPT and FRAMEWORKS and COOKIES and all these things, can seem extremely complicated and fancy. And who's that Tim Berner's Lee guy, why do we say he created the internet (spoilers: that's hyperbole!). It appears a technical, sophisticated monstrosity ordinary folks like us can't hope to understand; Our laptops send out radiowaves and somehow through this intricate system a webpage loads on our screen in milliseconds. If we step back a bit though and look at the earlier, less sophisticated days of "internet", the picture becomes clearer.

Before "the internet" there were *internets*, emphasis on the plural. And before internets, there were ... just computers connected to other computers. Actually this idea of connecting one computer to another goes back to the very start of computing. Remmember: Computers used to be *huge* and extremely expensive! They bassicly needed their own room. It was essential then that the one computer in the one room could be interfaced with through various interfaces/terminals throughout the building. The first of these interaction devices was a so-called "teleprinter", which was more or less just a traditional typewriter (think the old loud clicky device!) hooked up to a signal so that it could write itself onto paper, and it's input hooked up to a signal so that they could be sent to the central computer: 

**Picture of a teletyper here**

So you typed a command to the central computer, and it would send back a signal that litteraly printed out the result on a piece of paper in front of you. Here's some early ASCII art produced this way:

**The early teleprinter ASCII art**

Eventually the interfaces people used to communicate with the central computer turned into screens and keyboards, more similar to what we know from today. The point here is though, that from a very early point, computers *sort of* had the hardware to send and recieve bits to other computers/and-or-devices. Remmember this is all we're doing at the end of the day: we're sending a bunch of `01001010101001`'s and computers interpret these 0's and 1's however they want, do some logic, then send back some other `0100100001110`'s. 

People started going further, as in, sending their data over longer distances. In October 29. 1969, Leonard Kleinrock attempted to send a message to Charley Kline 350 miles away, sent over a telephone line (which aren't magic, they're literal lines, really long wires!). He tried to write "login", but the process crashed halfway through, so he just sent "lo". He then tried again, meaning that the first three letters sent over a long-distance network were litteraly "lol". Fun facts aside, this was the beginning of the Arpanet.

**image of the arpanet access points here**

If I want to communicate with you through only 0's and 1's, then we need some system we use to interpret our messages. Otherwise its all random noise. If we want *many* computers communicating, we also want a standardized system so that everyone aren't doing their own things and we get a huge mess. Moreso than just a system for interpreting the actual 0's and 1's, we need a protocol for *when* we send *what* to each other. Communication on the Arpanet was *sort of* structured, but not nearly as structured and well-defined as it is on "the internet" today. The arpanet grew and developed significantly over two decades, and it was first in 1989 we got the complete "Internet protocol suite".

Central to the "Internet protocol suite" is HTTP: The Hyper Text Tranfer **Protocol**. That's more or less the thing that tells us what our 0's and 1's mean and how/when we send, recieve and request "things". HTTP is the thing Tim Berner's Lee made (though many refinements were made by the INTERNET ENGINEERING TASK FORCE, an honestly super epic name). The HTTP proposes that this "thing" we sent each other is *Hypertext*; Text that links to other texts. The format of this hypertext? HTML: Hyper Text Markup Language.

Central to HTTP was its simplicity; The whole thing could be written on a whiteboard, and you can see the original specification here: https://www.w3.org/Protocols/HTTP/AsImplemented.html. It more or less just says: "If you host a folder on your computer with some .HTML files you'd like to share, I can request one of these files by sending you "GET http:your-super-cool-address/path/to/your/file", and your computer will react to this message by indeed sending me `file.html`. If your computer runs a database, say a databse of food, and I'd like to search for an item in this database, I can do `GET http:your-super-cool-address?name=cheese&taste=strong`, ie append "?" plus some search parameters like `name=cheese` and `taste=strong` to my request.

Let's test this! Rick click any part of this website and click "inspect". You should now see a developer menu showing you how the website is built up. It also gives you a *console* you can use to execute commands. If you're on Firefox, it should look something like this:

** Picture of the firefox developer menu here **

Now paste the following into this console:

```js
fetch('https://www.w3.org/Protocols/HTTP/AsImplemented.html', {method: "GET"})
  .then(response => response.text())
  .then(data => console.log(data));
```
You don't have to understand this completely, but as you can see, we're making a "GET" request to a `.html` file on this website, and indeed it just sents us the contents of the file. This is what your web browser does behind the scenes when you write in the link. It retrieves this text, interprets it, and renders it out in a graphical way with a certain visual structure. 

Now lets try making a slightly more sophisticated request by asking the openfoodfacts.com database to send us some information. Try pasting the following to your console:
```js
fetch("https://world.openfoodfacts.org/cgi/search.pl?search_terms=breakfast%20cereals&page_size=20&json=true", {
  method: "GET"
})
  .then((response) => response.json())
  .then((json) => console.log(json));
```
You should hopefully now see an "Object" that you can open up to see a list of "products" that match your search criteria:

** image here of the response you get **

Let's break down what's happening here a bit more. We request the address "world.openfoodfacts.org", specifically its search engine at the path "/cgi/search.pl", using of course the "HTTPS" (http-secure) protocol. We add the "?" to give it a list of parameters to use:
```
search_terms=breakfast%20cereals
&
page_size=20
&
json=true
```
We can't write spaces directly, so we use "%20" instead of space, which is a little confusing. All in all: We search for "breakfast cereals", we want 20 results, and we want it to format it in JSON (a format javascript can work with). 

In the original HTTP there was only GET requests, but it has since grew to 
other types of request like POST (for sending information *to* someone) and PUT and DELETE, plus a few others. We mostly just use GET for getting stuff and POST for adding/modifying stuff. GET is always the assumed default if you don't specify your type of action.

Note that the website (which is just a computer ... ) that you send a request to can essentially just do whatever it wants with these parameters; It doesn't neccesarily have to be limited to querying a database, and there is no universal standard for how to query the databases; It's up to the individual organization/website to specify a so-called API: Application Interface. The API tells us what addresses we should request with what parameters to do what things. Spotify has an API, youtube has an API, a whole bunch of organizations have API's, and most of them work through GET requests like these (it frequently gets more complicated because of authorization and security though ... ). 

# So how do we make basic websites?
Here's an incredibly simple website where the user can press a button and view some information about an item with a given barcode:
```html
<!doctype html>
<html lang="en-US">
  <body>
	  <!-- CONTENT (HTML) -->
	  <p>Hello, i am a paragraph!</p>
	  <p>There'll be some spacing between paragraphs, as you'd expect</p>
	  <p>Though if you try to just add spacing yourself inside a paragraph like this:

	  Then it doesn't appear! HTML doesn't care about the spacing in your text document like that</p>
	  <h1> I am header! </h1>
	  <h2> There's </h2>
	  <h3> different </h3>
	  <h4> levels of </h4>
	  <h5> header ... </h5>

	  <div>
		HTML elements can be either <i> block level </i> or <i> inline-level </i>.
		Block-level elements start a new line/break up the flow, while inline-level elements are in-line with the rest of the text.
		This "div" we're in right now is a block-level element, the italics tag <i></i> is a inline level element.
		<p>
			We can give elements ID's and classes that can be used in our CSS and Javascript. With that, we can
			do cool things like this <span class="my-cool-class"> red text </span> here.
		</p>
	  </div>

		<p>
		"div" is the most generic block-level container, and "span" is the most generic inline-level container. 
		There's a lot of elements to play around with in HTML, and you can find a list <a href="https://developer.mozilla.org/en-US/docs/Web/HTML/Reference/Elements"> at this link</a>.
		</p>

		<div>
			Type a barcode: <input id="barcode-textbox" type="text">.
			<div>
				<button id="search-button"> Search information </button>
			</div>
			<div id="my-information-container">
			The information should appear here once you press the button and the request is done : ) .
			</div>
		</div>

<!-- STYLING (CSS) -->
<style>
.my-cool-class {
	color: red;
	background-color: blue;
	text-decoration: underline;
	cursor: pointer;
}
</style>

<!-- FUNCTIONALITY (JAVASCRIPT) -->
<script>

// Get some elements we need:
let barcode_field = document.getElementById("barcode-textbox");
let search_button = document.getElementById("search-button");
let result_container = document.getElementById("my-information-container");

// Define some functions
function display_found_item(item_information) {
	result_container.innerHTML = "Found item! Item keywords: " + item_information.product._keywords;
}

function item_couldnt_be_found() {
	result_container.innerHTML = "Sorry, item couldn't be found :(";
}

function do_search() {
	let barcode = barcode_field.value;
	console.log("barcode is: " + barcode);

	// fetch information from server
	fetch("https://world.openfoodfacts.net/api/v2/product/" + barcode + ".json", {
	  method: "GET",
	  headers: { Authorization: "Basic " + btoa("off:off") },
	})
	// when it is retrieved, *then* we proceed to decode it
	.then(response => response.json() )
	// when it is decoded *then* we proceed to call our display_search() function with the decoded data
	// if there was an error, we call another function instead
	.then(display_found_item, item_couldnt_be_found);
}

// Say what you want to happen when the user clicks the button
// try searching for this id: 3274080005003
search_button.addEventListener("click", (e)=>do_search());

</script>

  </body>
</html>
```
You can copy-paste this HTML into a file, name it something like "my-cool-website.html", then open the file with your browser (right-click and press "Open with"), and you should now see the website in your browser. Amazing!

This website is made of three parts: First the actual contents (the HTML), then the styling (the CSS), then the functionality (the javascript).

Any program that can edit text can make a website (at least a simple one like this ... ). Or, well, any *text editor* can help you write websites; Tools like Word and Google Docs don't actually let you edit the file itself.

A lot of people will tell you that making websites is a very complicated involved process where you need all sorts of tools, but you really don't! Try to spend more time writing and understanding than you do setting up tools that help do the writing and understanding for you :). It *is* involved and takes a great deal of time *to do well*, but the basic thing doesn't require many tools at all.

For casual purposes, I'd reccomend Visual Studio Code as a simple and easy text editor, that will do some basic error highlighting for you. Its a wonderful program. 

I don't actually have time to explain much HTML, CSS or Javascript here, but you can find all the information you need, both introductory and in-depth, on the excellent Mozilla docs:
- [List of HTML elements](https://developer.mozilla.org/en-US/docs/Web/HTML/Reference/Elements)
- [Introduction to CSS](https://developer.mozilla.org/en-US/docs/Web/CSS)
- [List of CSS attributes](https://developer.mozilla.org/en-US/docs/Web/CSS/Reference)
- [Introduction to Javascript](https://developer.mozilla.org/en-US/docs/Web/JavaScript)

I would *highly* reccomend reading a bit from these docs instead of googling every single individual question you have. There's so much utter garbage out there on the internet, especially when it comes to technical questions, and especially when its something where money is involved like websites. Mozilla docs are so excellent you really don't need to spend a bunch of time surfing around for some forum post that answers exactly your question.

There's *a lot* of very unintuitive and strange things in both HTML, Javascript, and *especially* in CSS. It's not just you: There's genuinely some bonkers and strange decisions here that can't be changed now as it would break older websites.

A reccomend using a chatbot to help you understand code. If you ask it to explain things, it's genuinely fantastic for helping you figure out what to do and how to troubleshoot a particular situation. But *if* you try to let it make all the code for you, then you won't understand what you're doing, and you'll eventually end up on such a shaky foundation that you can't finish the rest of the project, so I highly reccomend not mindlessly copy pasting a bunch of code.

# The openfoodfacts API 
There's a general introduction here:
https://openfoodfacts.github.io/openfoodfacts-server/api

You can find some information about searching for items in the database here:
https://openfoodfacts.github.io/openfoodfacts-server/api/tutorials/finding-healthy-cereals/

This article jumps off to multiple other articles that show common use-cases of the API:
https://openfoodfacts.github.io/openfoodfacts-server/api/tutorial-dev-journey/
