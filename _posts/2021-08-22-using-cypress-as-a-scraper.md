---
layout: post
title:  "Using Cypress as a Web Scraper"
date:   2021-08-10 1:00:00 -0500
categories: cypress
---

<div class="post-image" style="text-align: center;">
  <img src="{{site.baseurl}}/assets/img/scraper.png">
</div>
<br/>

Cypress is a tool specially made for testing. So using cypress for web scraping does not look like a good usage case. Maybe there are better tools like python **beautifulsoup**, or **puppeteer**. But let's break things and try to scrape Maroon 5 songs lyrics and save them in a file.

Let's try this spec to visit the Google search page and type some Maroon 5 songs titles. Then we can capture the text and save it to file. There is no need to use nodejs writefile command because Cypress comes with the command **cy.Writefile()**. We take advantage of the fact that the HTML source code is pretty consistent no matter the song title and the language it is written in.

```
const songs = Cypress.env('maroonSongs');

describe('View Maroon 5 songs', () => {
    beforeEach(() => {
      cy.visit('https://www.google.com/')
    })
    
    for (let song of songs) {
        it('Google search', () => {
            cy.get('[aria-label="Buscar"]')
                .type(`${song.name}{enter}`);

            cy.get('[jsname="WbKHeb"]').within(($val) => {
                cy.get('[jsname="YS01Ge"]').each(($el, index, $lista) => {
                    let linea = $el.text() + '\n';
                    cy.log(linea);
                    cy.writeFile(`${song.filename}`, linea, { flag: 'a+' });
                });
            });
        });
    }
});
```

The list of songs and the files to store them are defined in cypress.json:
```
{
    "env": {
        "maroonSongs": [
            {"name": "girls like you lyrics", "filename": "girls.txt"},
            {"name": "she will Be Loved lyrics", "filename": "shewill.txt"},
            {"name": "moves like Jagger lyrics", "filename": "jagger.txt"}
        ]
    },
}
```

After running the script we get three files with the right content:

```
- girls.txt
- jagger.txt
- shewill.txt
```

A use case for this kind of web scraper is to get data to train a natural language processing model using Tensorflow or any other NLP tool.