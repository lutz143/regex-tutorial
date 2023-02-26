# Regex Price Webscraping Tutorial

#### **<ins>Note:</ins>** The regex code explained in this tutorial was developed by myself and works on a test HTML document I also created. Any application of this regex should be thoroughly tested.

## Intro
The Regex Price Webscraping Tutorial is a tutorial that explains how a webscraping regular expression, or regex, finds the price of a product on a dummy website.

## Summary
The regex code I will be describing is a code that webscrapes an HTML document that searches for and returns the price of a product of a test page.  This regex operation would be helpful for a company who is looking to compare the price of their products to a competitor or for the average consumer to create an automated alert for when the price of a product goes on sale.

In this tutorial, I will break down the different components of the price webscraping regex operation, what identifiers are necessary to complete the operation such as grouping and back-references, and how it may be used.

---
Regrex Code: ```(?<=id="price\s?"\n?.*)(?<=>\$)(\s?\n?.*)(\d)```

---

![image](/Develop/img/RegexSnipIt.PNG)

---


## Table of Contents


- [Quantifiers](#quantifiers)
- [Character Classes](#character-classes)
- [Flags](#flags)
- [Grouping and Capturing](#grouping-and-capturing)
- [Greedy and Lazy Match](#greedy-and-lazy-match)
- [Lookahead and Lookbehind](#lookahead-and-lookbehind)
- [About the Author](#about-the-author)


## Regex Components

### Quantifiers
* The dot star `.*` quantifier is utilized on two occassions in this regex code.
* The dot `.` matches any character excluding any linebreaks within the search.
* The star or asterisk `*` returns any characters after the preceding token.
* Taken together, the dot star `.*` quantifier returns anything after its identified search parameter. Meaning, in our case, the first grouping will find all of the remaining HTML element after the positive lookbehind is performed that finds any HTML element with an id of "price". 
* By creating boundaries on the dot star quantifier, we may find the HTML element that contains the product's price and only return the inner text that contains the numeric digits of the price.

### Character Classes
* The price regex utilizes four groupings to capture the inner text of the HTML element that is a numeric digit. 
* The first two groupings are lookbehinds capturing the correct HTML tag related to a product's price and focusing on the inner text of the element. See the [Lookahead and Lookbehind](#lookahead-and-lookbehind) section for more information on how these work.
* The third grouping `(\s?\n?.*)` identifes the number of characters to capture in the HTML's inner text. Here, the regex grouping cannot simply perform a search on a numeric digit (e.g. `\d\d`) as a price may contain many digits (i.e. 4.00, 40.00, 4,000.00). Therefore, this grouping is necessary to capture any leading whitespace (`\s`), any inadvertant line breaks (`\n`), and the remaining digits in the inner text of the HTML element by utilizing the dot star quantifier `.*`. The whitespace and line break regex operators are followed by the lazy match `?` to confirm these search parameters are matched with as few occurrences as possible.
* The fourth and final grouping `(\d)` is performed to create the remaining boundary to limit the return value to only what is within the HTML's inner text. By limiting the group's capture to any digit character (0-9), the regex is ensuring that any remaining HTML information, such as closing tags or additional elements, are excluded from the result as those will be text characters and thus will not be captured.

### Flags
* The global flag at the end of the regex code `/g` is required to retain the index of the last match of the search parameter. As a result, the global search flag will retain subsequent searches from the end of the previous match.
* Without the global flag, each search grouping in the regex will not have a starting point for which additional parameters and boundaries are created.
* An example as to why this is important is with the first two groupings where a positive lookbehind is performed. Without the global flag, the regex would find only a single match of the positive lookbehind (in our example the HTML element that has the id of "price"). Therefore, any subsequent components would be ignored and we would not be able to limit our result to the inner text of the HTML element.

### Grouping and Capturing
* There are four grouping constructs in the webscraping price regex. Each of these groupings performs a specific operation in order to break down the code into subexpressions.
* The first grouping construct is a positive lookbehind that is searching for any HTML element with the id of "price".
* The second grouping construct is another positive lookbehind that creates a boundary on the first group to end the dot star (`.*`) search so that the regex capture begins after the opening HTML tag is closed with a `>`.
* The third grouping creates lazy matches on any whitespace or linebreaks and captures the remaining information after the opening HTML tag is closed with a `>` by including another dot star (`.*`) search.
* The fourth grouping creates another boundary on the third group so that only numeric digits are captured and closing HTML elements are ignored.

### Greedy and Lazy Match
* Quantifiers are inherently greedy so that there are as many matches as possible. For instance, two greedy quantifiers are used in this regex with the dot star (`.*`) methodology. By including these two greedy matches, the regex will return all of the subsequent information of a particular line upon which the group has matched on.
* Lazy matches are also used in this regex so that the preceding token matches as few times as possible. For example, several lazy quantifiers are used when searching for whitespace and/or new line breaks. This is important when there may be inadvertant or a break from the normal naming convention of the HTML document. You'll notice in the dummy website, that there is one HTML element that has a space after the id="price ". With the lazy match, the regex will capture both id="price" as well as id="price " to ensure performance on both instances.

### Lookahead and Lookbehind
#### First Lookbehind
* The regex utilizes a positive lookbehind as one of the first components of the operation. `(?<=id="price\s?"\n?.*)`
* Here, within the first grouping, the user will notice the `?<=` signaling to match the HTML tag of `id=price` without including it in the result.
* Within the lookbehind, the id=price has several lazy matches (as explained above) to ensure any unique naming conventions or errors in the HTML tag are caught. The regex checks to see if there are any added spaces in the id name or a new line was added inadvertantly.
* Next, the positive lookbehind captures the remaining elements/characters of the HTML line (excluding line breaks) by including the dot star quantifier `.*`
#### Second Lookbehind
* Another positive lookbehind is used in the second grouping to create a boundary of what is captured in the first positive lookbehind grouping. `(?<=>\$)`
* This second lookbehind is important as it tells the regex to exclude the elements/characters that were captured after the dot star quantifier in the first lookbehind. By binding what is excluded after the opening HTML element is closed and the USD currency is used `>\$`, the regex is now removing the preceding text in the HTML options to focus only on the inner text.

## About the Author

* To contact the author of this tutorial, you may reach him on GitHub: [lutz143](https://github.com/lutz143)

* Josh Lutz is currently a manager at Lockheed Martin, leading Special Projects and the creation of an Alteryx-Tableau database. He's had the opportunity to work in unique positions and multiple locations awarding him the ability to develop several applications utilizing a suite of programming languages. This experience has provided him a framework to gather data, identify pertinent information, and evolve as a full stack developer.
