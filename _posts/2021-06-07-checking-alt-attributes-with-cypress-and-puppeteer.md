---
layout: post
title:  "Checking alt attributes with Cypress and Puppeteer"
date:   2021-06-07 9:10:00 -0500
categories: cypress
---

<div class="post-image" style="text-align: center;">
  <img src="{{site.baseurl}}/assets/img/wiki-cypress.png">
</div>

Accessibility is a practice intended to make Web pages reachable for people with dissabilities, poor network connections, or people with cheap mobile devices. The alternate text in images makes possible for visually impaired people to get the context using screen readers. Making sure that all images in a Web page have alt text is a way to check this accessibity requirement.

Here we are going to check that all images in a Wikipedia page have the alt attributes using Puppeteer and Cypress to compare which code could be more convenient and efficient.

### Checking alt attributes using Puppeteer and Mocha

{% highlight javascript %}
const puppeteer = require('puppeteer');
const expect = require('chai').expect;

describe('Visit wikipedia landing page in Spanish', () => {
    let browser;
    let page;
    before(async() => {
        browser = await puppeteer.launch();
    });
    
    beforeEach(async() => {
        page = await browser.newPage();
    });
    
    afterEach(async() => {
        await page.close();
    });

    after(async() => {
        await browser.close();
    });

    it('should have anchors with images', async() => {
        await page.goto('https://es.wikipedia.org/wiki/Wikipedia:Portada');
        expect(await page.title()).to.contain('Wikipedia');
        const imagenes = await page.$$('img');
        for (let i = 0; i < imagenes.length; i++) {
            expect(await imagenes[i].evaluate((node) => node.getAttribute('alt'))).to.be.a('string');
            let altText = await imagenes[i].evaluate((node) => node.getAttribute('alt'));
            console.log(altText);
        }
    })
})
{% endhighlight %}

### Checking alt attributes using Cypress

{% highlight javascript %}
/// <reference types="cypress" />

const URL = 'https://es.wikipedia.org/wiki/Wikipedia:Portada';

describe('Testing Wikipedia', () => {
  beforeEach(() => {
    cy.visit(URL);
  });
    
  it('Get alt from images', () => {
    cy.title().should('contain', 'Wikipedia');
    cy.get('img')
      .then((elems) => {
        for (const elem of elems) {
          cy.wrap(elem).invoke('attr', 'alt').should('be.a', 'string')
            .then((altText) => {
              console.log(altText);
            })
        }
    });
  });
});
{% endhighlight %}

Both tests take around 12 seconds which is not a significant difference. The code using Cypress is shorter because there is no need to use as many hooks as used in Puppeteer-Mocha.