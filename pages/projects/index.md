There are projects I worked on and wanted to write something about. From newest to oldest.

## External Web Interface for Suppliers

<span class="f6 link br3 ph3 pv1 mb2 dib blue bg-lightest-blue">1C:Enterprise</span> <span class="f6 link br3 ph3 pv1 mb2 dib blue bg-lightest-blue">React.js</span> <span class="f6 link br3 ph3 pv1 mb2 dib blue bg-lightest-blue">Flask</span>

Our customer was the owner of a trading company. Part of its activity is pricing: employees of the company contacted suppliers, determining which of them was more profitable to place customer orders considering prices, delivery times and other conditions.

We were required to make it so that they could simply send suppliers links to a web interface where they could enter prices and answer additional questions.

The platform's web interface could not be used due to license savings. Therefore, I wrote a React.js web application that works with the 1C:Enterprise database through a REST interface specially designed for this.

Have a [look](https://github.com/vkostyanetsky/RFQ) at project's demo on my GitHub.

## UAC Currency Rates Crawler

<span class="f6 link br3 ph3 pv1 mb2 dib blue bg-lightest-blue">1C:Enterprise</span> <span class="f6 link br3 ph3 pv1 mb2 dib blue bg-lightest-blue">Flask</span> <span class="f6 link br3 ph3 pv1 mb2 dib blue bg-lightest-blue">MongoDB</span>

We wanted to give FirstBit ERP users the ability to automatically receive dirham exchange rates published by the UAE Central Bank website.

The site parsing code could be added to the configuration itself, but this idea seemed to be problematic from several points of view: there is no convenient mechanism for obtaining rates on the Central Bank website, the site itself is unstable, and it is difficult to change the parsing logic in many databases in case we need to.

Therefore, I wrote a Python console application that regularly parses published exchange rates to a special database and sends them through a REST service. We deployed this application on our server, and I added only regular requests to the service on FirstBit ERP side.

Have a [look](https://github.com/vkostyanetsky/CurrencyRatesCrawler) at project's backend code on my GitHub.