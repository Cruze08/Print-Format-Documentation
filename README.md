#                                        PRINT FORMAT

## Understanding in creating print format

### Creating Print Format in Frappe

#### Frappe allows you to create customized print formats for your documents. A Print Format defines how your document (such as an Invoice, Delivery Note, or Sales Order) is displayed when printed or converted to PDF.
#### This guide will walk you through the steps for creating and customizing a Print Format in Frappe.

## Steps to Create a Print Format

### 1. Accessing the Print Format List

#### To create a new Print Format:
#### • Go to the Desk.
#### • Navigate to Awesome Bar> Print Format.
#### • Click on New.


### 2. Setting Up the Basic Details

#### In the new Print Format form, you will need to fill in some basic information: 

#### Name: A unique name for your print format.
#### DocType: The DocType for which this print format is being created (e.g., Sales Invoice, Delivery Note).

#### • Format Type: Choose the format type. Options include:
#### ◦ Standard: Use predefined templates and customize fields through the UI.
#### ◦ HTML: Customize the print format using HTML and Jinja templating.

#### Letterhead: If you want to include a company letterhead, enable the Letterhead field.

#### Custom Format: Check this box if you want to use your own custom HTML code.



### 3. Designing the Print Format

#### There are two ways to design your Print Format:

#### 3.1 Using the Standard Editor

#### If you are not using the Custom Format option, Frappe provides a simple interface to add, remove, or rearrange fields. Follow these steps:

#### Under Fields, select which fields from the DocType you want to display.
#### Adjust the sequence and labels as necessary.
#### Define custom properties like bold, italic, and text alignment for each field.


#### 3.2 Using Custom HTML (Custom Format)

#### For advanced customization, check the Custom Format box, which allows you to write custom HTML and CSS for the Print Format.

#### You can use the built-in Jinja templating engine to fetch and display data dynamically.
#### Insert fields using Jinja expressions like {{ doc.fieldname }}.
#### You can also loop over child tables, apply conditions, and format data.

### Example:

```
<div>
    <h3>{{ doc.company }}</h3>
    <p><strong>Customer Name:</strong> {{ doc.customer_name }}</p>
    <p><strong>Date:</strong> {{ doc.transaction_date }}</p>

    <table class="table table-bordered">
        <thead>
            <tr>
                <th>Item</th>
                <th>Qty</th>
                <th>Rate</th>
                <th>Total</th>
            </tr>
        </thead>
        <tbody>
            {% for row in doc.items %}
            <tr>
                <td>{{ row.item_name }}</td>
                <td>{{ row.qty }}</td>
                <td>{{ row.rate }}</td>
                <td>{{ row.amount }}</td>
            </tr>
            {% endfor %}
        </tbody>
    </table>
</div>
```

### 4. Jinja Template Engine

#### Frappe supports the Jinja templating engine, which allows you to perform calculations, loop over child tables, and apply logic. Below are some common examples:

#### 4.1 Fetching Values:
```{{ doc.customer_name }}  <!-- Fetches the customer name -->
```
#### 4.2 Looping Over Child Tables:
```
{% for row in doc.items %}
    <tr>
        <td>{{ row.item_name }}</td>
        <td>{{ row.qty }}</td>
    </tr>
{% endfor %}
```

#### 4.3 Conditional Statements:
```
{% if doc.total > 10000 %}
    <p>This is a large transaction.</p>
{% endif %}
```


### 5. Adding Custom CSS

#### You can add custom styles using inline CSS or by referencing external stylesheets within your print format HTML. For example:

```
<style>
  .custom-header {
      font-size: 16px;
      font-weight: bold;
  }
  .table {
      border-collapse: collapse;
      width: 100%;
  }
  .table, .table th, .table td {
      border: 1px solid black;
  }
</style>
```

### 6. Using Functions

#### Frappe provides a set of utility functions that can be used in Print Formats.  

### Example: Convert a number into words

```<p>Amount in words: {{ frappe.utils.money_in_words(doc.grand_total) }}</p>
```

### 7. Preview and Test

#### After saving your Print Format, go to any document of the selected DocType.
#### Click on Menu > Print to see the document with your newly created Print Format.
#### Ensure all fields are displayed correctly and calculations or conditions work as expected.


### 8. Assign the Print Format  
#### To assign your new Print Format as the default:
#### Go to Customize Form.
#### Select the relevant DocType.
#### In the Print Settings section, set the default Print Format.
#### Alternatively, you can select the Print Format manually at the time of printing.


### 9. Common Use Cases

#### nvoices: Customize the layout to display customer details, itemized charges, taxes, and payment information.
#### Purchase Orders: Display vendor information, item details, and terms of purchase.
#### Delivery Notes: Include shipment details, customer signature areas, and tracking numbers.


### 10. Best Practices
#### Use tables to align data for professional layouts.
#### Leverage Frappe utilities like money_in_words to handle currency formatting.
#### Ensure the layout is printer-friendly and adjust margins for better print results.
#### Test different browsers to ensure cross-browser compatibility of the Print Format.


### 11. Troubleshooting
#### Fields Not Displaying: Check if the field names in the HTML match exactly with the DocType fields.
#### Broken Layout: Ensure your HTML and CSS are properly structured, especially when using complex tables or custom styling.
#### Data Not Updating: Confirm that the Print Format is saved and that the document is refreshed when testing changes.


## Page Break in Print Format in Frappe
## Steps to Add a Page Break

### 1. Using HTML for Page Breaks
### Page breaks in print formats can be added using simple HTML. The <div> tag can be styled with the page-break-before or page-break-after CSS properties to control when a new page should start.

#### Example of adding a page break in a custom print format:

```
<div style="page-break-before: always;"></div>
```
#### This will ensure that whatever content follows the <div> tag will start on a new page.

#### You can also use:
```
<div style="page-break-after: always;"></div>
```

#### This will cause a page break right after the content before the <div> tag, starting the next content on a new page.



### 2. Example in a Print Format

#### Here’s an example of a simple print format that uses a page break:
```
<h2>Section 1</h2>
<p>This is the first section of the document. It will be printed on the first page.</p>

<div style="page-break-before: always;"></div>

<h2>Section 2</h2>
<p>This is the second section of the document. It will be printed on a new page.</p>

<div style="page-break-after: always;"></div>

<h2>Section 3</h2>
<p>This is the third section, and it will again be printed on a new page.</p>
```

#### In this example:
#### Section 1 will appear on the first page.
#### Section 2 will start on a new page due to page-break-before: always.
#### Section 3 will also start on a new page due to page-break-after: always.

### 3. Adding Page Breaks in Jinja-Based Print Formats
#### If you’re using Jinja templating for your print format, you can still include page breaks using HTML and CSS.

#### Example using Jinja in a print format:
```
<h1>{{ doc.customer_name }}</h1>
<p>Customer Address: {{ doc.address }}</p>

<table>
  <tr>
    <td>Item</td>
    <td>Quantity</td>
    <td>Rate</td>
    <td>Amount</td>
  </tr>
  {% for item in doc.items %}
  <tr>
    <td>{{ item.item_name }}</td>
    <td>{{ item.qty }}</td>
    <td>{{ item.rate }}</td>
    <td>{{ item.amount }}</td>
  </tr>
  {% endfor %}
</table>

<div style="page-break-before: always;"></div>

<h2>Additional Details</h2>
<p>{{ doc.terms_and_conditions }}</p>

```
#### In this case, after printing the table of items, a page break will occur, and the "Additional Details" section will be printed on a new page.

#### When to Use Page Breaks
#### Long Tables or Data: If your document contains long tables or sections of data, it's often better to break them into multiple pages to ensure clean printing.
#### Separate Sections: Use page breaks to clearly distinguish between different sections of your document (e.g., customer details, product list, terms, etc.).
#### Legal Documents: In legal or contract-based documents, it may be important to control exactly how content flows across pages, making page breaks necessary.





# Doubts in Nani
### In the Nani print format, I’ve come across a specific block of code that implements a page break when the number of items in a document reaches 15. Here’s the part of the code:
```
{% if doc.items | length == 15 %}
  <tr class="page-break"></tr>
{% endif %}
```

### {% if doc.items | length == 15 %}:
#### This is a Jinja condition that checks the number of items (doc.items) in the document.
#### doc.items is a list of items in the document, and | length is a Jinja filter that returns the total number of items.
#### If the length of doc.items equals 15, the condition becomes true, and the following code is executed.

### <tr class="page-break"></tr>:
#### This is an HTML table row (<tr>) with a CSS class called page-break.
#### The purpose of this line is to insert a table row with a page break when the condition is met (i.e., when there are exactly 15 items).
#### The page-break class is most likely defined in the CSS to force a page break at that point.


### In the Nani print format, there's a portion of the code that I'm still trying to fully understand. It seems to be related to setting dynamic page breaks based on the number of items in the document. Here's the part I'm struggling with:

```

{% set b = frappe.get_doc("Delivery Note", posting_date, tc_name, vehicle_no) %}

{% if doc.items | length <= 5 %}
  {% set rows_before_break = 0 %}
{% elif doc.items | length == 6 %}
  {% set rows_before_break = 10 %}
{% elif doc.items | length == 7 %}
  {% set rows_before_break = 10 %}
{% elif doc.items | length == 8 %}
  {% set rows_before_break = 9 %}
{% elif doc.items | length == 9 %}
  {% set rows_before_break = 8 %}
{% elif doc.items | length == 10 %}
  {% set rows_before_break = 7 %}
{% elif doc.items | length == 11 %}
  {% set rows_before_break = 6 %}
{% elif doc.items | length == 12 %}
  {% set rows_before_break = 5 %}
{% elif doc.items | length == 13 %}
  {% set rows_before_break = 3 %}
{% elif doc.items | length == 14 %}
  {% set rows_before_break = 2 %}
{% elif doc.items | length == 15 %}
  {% set rows_before_break = 0 %}
{% endif %}

```

### My Current Understanding:
### {% set b = frappe.get_doc("Delivery Note", posting_date, tc_name, vehicle_no) %}:

#### This line seems to be fetching a Delivery Note document using frappe.get_doc(), with the parameters posting_date, tc_name, and vehicle_no. I'm guessing these are variables related to the document, but I’m unclear about how these parameters are passed or used.

### Setting rows_before_break Based on doc.items Length:
#### The code then checks the number of items in the document using doc.items | length. Based on the number of items, a variable called rows_before_break is set to a specific value.
#### For example, if there are 6 items, rows_before_break is set to 10; if there are 15 items, it’s set to 0.

#### I assume this variable is used to control where the page break occurs based on the number of rows, but I'm not sure how the specific values (like 10, 9, 8, etc.) are chosen. Is there a logic behind the numbers for the rows before a break?


### 3. Page Break Logic:
#### After setting rows_before_break, there's a loop (starting at <tbody>) where row_counter is incremented after each row:
```
 {% set row_counter = row_counter + 1 %}
            {% if row_counter == rows_before_break %}
                     <tr class="page-break"></tr>
            {% endif %}
```

#### I think this part is responsible for inserting a page break when the number of rows printed reaches the value of rows_before_break. But again, I'm unsure how the value of rows_before_break is calculated based on the length of doc.items.

### Our Questions:

#### 1. What is the reasoning behind the specific values (like 10, 9, 7, etc.) for rows_before_break based on the length of doc.items?
####     2. Is this part of the code dynamically controlling the page breaks? If so, how exactly does this dynamic page-break mechanism work?


## Doubts in Paras Print Format:
### In the Paras Customer Datasheet print format, I noticed that you didn’t explicitly add any page break code in the HTML, such as <div style="page-break-before: always;"></div>. However, the data is still printing with proper page breaks, and I’ve understood how this is happening.

### What I Want to Learn:
#### I want to understand more about how to control these automatic page breaks in scenarios where the content is long or where specific sections need to stay together on a single page. Even though the browser does a good job of automatically handling page breaks, there may be cases where we need more control.
#### In this context, I would like to learn:

#### How to force page breaks or prevent breaks between certain sections.
#### How to handle large tables or images that span multiple pages.
#### Any best practices for ensuring that the automatic page breaks look clean and professional in every document.



#### In the Nani print format, I understand how to implement page breaks for tables, but I want to extend my knowledge to handle cases where the field, like {{ doc.description }}, contains only textual data (not a table). Specifically, I’m curious about how to:
#### 1. Automatically Insert a Page Break for Overflowing Text: If the content in {{ doc.description }} overflows to the next page, I want to make sure a clean page break occurs. Since the description field may contain a lot of text, it could spill onto the next page.
#### 2. Adding Fixed Padding When Content Starts on a New Page: In cases where the content from {{ doc.description }} starts on a new page, I want to add a fixed padding to ensure that the text doesn’t appear cramped at the top of the new page. This padding helps improve readability and creates a clean layout.

#### Layout Difference Between PDF and Print in Print Formats
#### One thing I noticed while working with page breaks in the all  print format is that the layout behaves differently when I use the PDF button compared to when I use the PRINT button.


## Issue with Repeating Header and Footer in Print Format
#### I have successfully implemented the functionality to repeat the header and footer on each page in the print format using CSS. However, I am encountering an issue where the content of the next page overlaps the repeated header. Here's a breakdown of what I did and the problem I'm facing:

### What I Did:
#### 1 Implemented Repeating Header and Footer:
#### I used CSS and HTML to ensure that the header and footer sections are repeated on each page when the document is printed.
#### For the header, I used the @media print CSS rule to make the header repeat on each page, and similarly for the footer.
#### Example:
```
<div class="header">
   <!-- Header content here -->
</div>

<div class="footer">
   <!-- Footer content here -->
</div>

<style>
   @media print {
      .header {
         position: fixed;
         top: 0;
         width: 100%;
      }
      .footer {
         position: fixed;
         bottom: 0;
         width: 100%;
      }
   }
</style>
```

#### 2. Page Breaks:
#### I also made sure to implement page breaks using the page-break-before and page-break-after properties to ensure that content flows properly across pages.

#### The Problem:
#### When the header and footer repeat on each page, the content of the next page overlaps with the header at the top of the page. This happens because the body content is not properly offset by the height of the repeated header, causing an overlap.

#### What I Think is Happening:
#### The fixed positioning of the header and footer ensures that they remain at the top and bottom of the page, respectively. However, the body content does not account for the height of the header, which leads to the overlapping issue.
#### Essentially, while the header and footer are fixed in place, the body content is not being pushed down enough to avoid the header.