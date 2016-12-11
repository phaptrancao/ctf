A CTF writeup to read And how to write **Markdown**

I especially thanks to this [blog](https://blog.ghost.org/markdown "https://blog.ghost.org/markdown")

# Basic Markdown Formatting
## Heading
  \# Heading 1
  
  \#\# Heading 2
  
  \#\#\# Heading 3
  
  ... and so on up to 6
 
## Text
  \*italic\*
  
  \*\*bold\*\*
  
  \*\*\*bold-italic\*\*\*
  
  \[link\](http://example.com)
  
  \~~stright through~~
  
  \`inline code\`

## Images
  \!\[m'lady](http://i.imgur.com/v8IVDka.jpg)
  
  Markdown images have exactly the same formatting as a link, except they’re prefixed with a !
 
## Lists

\* Milk

\* Bread

\* Wholegrain

\* Butter

\ 1. Tidy the kitchen

\ 2. Prepare ingredients

\ 3. Cook delicious things  

Lists are a formatting nightmare in HTML, but Markdown lists are incredibly easy to manage. For a bullet list, just prefix each like with a * - or - or + and they will be converted to dots. You can also create nested lists; just indent a line with 4 spaces and it will be nested under the line above.

  * Milk
  * Bread
      * Wholegrain
  * Butter

For numbered lists, do exactly the same thing - but use numbers!

## Quotes

  \> To be or not to be, that is the question.

When you want to add a quote in Markdown, it’s exactly the same as the formatting which you may already be familiar with from your email app of choice when you reply to someone.

> To be or not to be, that is the question.

Prefixing the line with a > converts it into a block-quote.

# Intermediate Markdown Formatting
## Horizontal Rules (The line that divide your article into different sections of text)
\---

Want to throw-down a quick divider in your article to denote a visual separation between different sections of text? No problem. 3 dashes \--- produce:

## Code Snippets
Some text with an inline \`code\` snippet  

\    .my-link {
\        text-decoration: underline;
\    }

Using a single **back-tick** around a word in a sentence, you can show a quick `code` snippet.

Indenting by **4 spaces** will turn an entire paragraph into a code-block.
## Reference Lists & Titles

\*\*The quick brown \[fox][1], jumped over the lazy \[dog][2].\*\*

\ [1]: https://en.wikipedia.org/wiki/Fox "Wikipedia: Fox"
\ [2]: https://en.wikipedia.org/wiki/Dog "Wikipedia: Dog"
