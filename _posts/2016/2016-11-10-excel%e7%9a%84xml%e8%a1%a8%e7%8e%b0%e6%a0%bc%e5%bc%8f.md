---
layout: post
title: Excel的xml表现格式
date: 2016-11-10 00:39
author: admin
comments: true
categories: [Uncategorized]
---
<div class="clsNoIndent"><span class="ArticleInlineTitle">With the soon-to-be released </span>next version of Microsoft<span class="superscript">®</span> Office (currently code-named "Office 12"), there will be new default file formats for Microsoft Word, PowerPoint<span class="superscript">®</span>, and Excel<span class="superscript">®</span>. These new formats, called the Microsoft Office Open XML Formats, will open up a whole new world to Office developers. By default, Office documents will be open and accessible, as they will use standard ZIP and XML technologies with full documentation made available under a royalty-free license. These technologies are an improvement on the existing XML formats that shipped with Microsoft Office 2003 Editions, but those existing Office 2003 XML Reference Schemas can be used today to implement solutions that work with the document data and they provide a great way to gain an understanding of what developing with the new default formats will entail.</div>
<div class="ArticleNormalPara">The SpreadsheetML format in Microsoft Excel is fairly easy to work with, as it was designed especially to be human readable and editable. But many of you probably haven’t had a chance to take a look at the XML support in Excel. Once you get a handle on how it works, though, you’ll realize you have plenty of uses for the XML features, from converting data between databases and Web pages to sharing files among disparate applications.</div>
<div class="ArticleNormalPara">To get you started, I’ll build a sample in XML that will illustrate how it all works. As you follow along, you can use Office XP or Office 2003 for this example since both support SpreadsheetML in their versions of Excel. Using a text editor, I’m going to create a very simple table that looks like <strong>Figure 1</strong>, outlining seven steps to create an XML file that represents an Excel worksheet.</div>
<div class="figmargin">
<div class="MTPS_CollapsibleRegion">
<div class="CollapseRegionLink">Figure 1 The Table Example</div>
<div class="MTPS_CollapsibleSection">
<table class="charttable ">
<tbody>
<tr>
<td class="clsChart">First Name</td>
<td class="clsChart">Last Name</td>
<td class="clsChart">Phone Number</td>
</tr>
<tr valign="top">
<td class="clsChart">Nancy</td>
<td class="clsChart">Davolio</td>
<td class="clsChart">(206) 555-9857</td>
</tr>
<tr valign="top">
<td class="clsChart">Andrew</td>
<td class="clsChart">Fuller</td>
<td class="clsChart">(206) 555-9482</td>
</tr>
<tr valign="top">
<td class="clsChart">Janet</td>
<td class="clsChart">Leverling</td>
<td class="clsChart">(206) 555-3412</td>
</tr>
<tr valign="top">
<td class="clsChart">Margaret</td>
<td class="clsChart">Peacock</td>
<td class="clsChart">(206) 555-8122</td>
</tr>
<tr valign="top">
<td class="clsChart">Steven</td>
<td class="clsChart">Buchanan</td>
<td class="clsChart">(71) 555-4848</td>
</tr>
</tbody>
</table>
</div>
</div>
</div>
&nbsp;
<div id="s1" class="ArticleTypeTitle">1. Create the XML File</div>
<div class="ArticleNormalPara">

To begin, create a new file in Notepad, and call it test.xml. Then follow the steps outlined here. First type the following:
<div id="ctl00_MTContentSelector1_mainContentContainer_ctl02_" class="libCScode">
<div class="CodeSnippetTitleBar"></div>
<div dir="ltr">
<pre class="libCScode">&lt;?xml version="1.0"?&gt;
</pre>
</div>
</div>
This declares that the file is an XML document adhering to the 1.0 version of the XML spec. It should always be found at the top of all your XML files. Next add the root element for the document. XML files always have one and only one root element that contains the rest of the document. For SpreadsheetML, the root element is &lt;Workbook&gt;. After the XML declaration, add that element so that your file now looks like this:
<div id="ctl00_MTContentSelector1_mainContentContainer_ctl03_" class="libCScode">
<div class="CodeSnippetTitleBar"></div>
<div dir="ltr">
<pre class="libCScode">   &lt;?xml version="1.0"?&gt;
   &lt;Workbook&gt;
   &lt;/Workbook&gt;
</pre>
</div>
</div>
</div>
&nbsp;
<div id="s2" class="ArticleTypeTitle">2. Declare the Namespace</div>
<div class="ArticleNormalPara">

Now you’ll declare the namespace and add a prefix to the root element. Most XML documents have a namespace associated with them. Declaring the namespace of an XML file makes it a lot easier for users parsing your XML to know what type of XML they are dealing with. Even in Office there are a number of different uses for XML. One way to know when you are parsing a Word XML file as opposed to an Excel XML file, for example, is to look at the namespace. With Office XP, when the product group created the SpreadsheetML schema, we were still using namespaces in the form "urn:schemas-microsoft-com:office". Going forward, we’ll use URL namespaces, as we did with WordML in Office 2003 (//schemas.microsoft.com/office, for example). By adding the namespace declaration to the spreadsheet, your file should look like this:
<div id="ctl00_MTContentSelector1_mainContentContainer_ctl04_" class="libCScode">
<div class="CodeSnippetTitleBar"></div>
<div dir="ltr">
<pre class="libCScode">&lt;?xml version="1.0"?&gt;
&lt;Workbook xmlns="urn:schemas-microsoft-com:office:spreadsheet"&gt;
&lt;/Workbook&gt;
</pre>
</div>
</div>
</div>
<div class="ArticleNormalPara">

The last thing you’ll do for the namespace is use a prefix, rather than the default. Since the attributes are qualified for the SpreadsheetML schema, you need to do this if you are going to use any attributes. Let’s use "ss" (for spreadsheet) as the prefix. You’ll add "ss:" in front of all of your elements, and you’ll update your namespace declaration to say that the namespace applies to everything with an "ss:" in front of it, instead of just applying to the default XML elements, as shown here:
<div id="ctl00_MTContentSelector1_mainContentContainer_ctl05_" class="libCScode">
<div class="CodeSnippetTitleBar"></div>
<div dir="ltr">
<pre class="libCScode">&lt;?xml version="1.0"?&gt;
&lt;ss:Workbook xmlns:ss="urn:schemas-microsoft-com:office:spreadsheet"&gt;
&lt;/ss:Workbook&gt;
</pre>
</div>
</div>
</div>
<div class="ArticleNormalPara">Notice that the namespace declaration says xmlns:ss= instead of just xmlns=. This means that anything with an "ss:" in front of it applies to the spreadsheet namespace.</div>
&nbsp;
<div id="s3" class="ArticleTypeTitle">3. Add a Worksheet</div>
<div class="ArticleNormalPara">

Next you’ll add a worksheet. Since you have an empty workbook, you need to declare the spreadsheet grid within the workbook. As you may know, workbooks can have multiple worksheets, but here you’ll just declare one. In addition, let’s declare a table inside the worksheet. The table is where all the grid data will go, and the file will now look like this:
<div id="ctl00_MTContentSelector1_mainContentContainer_ctl06_" class="libCScode">
<div class="CodeSnippetTitleBar"></div>
<div dir="ltr">
<pre class="libCScode">&lt;?xml version="1.0"?&gt;
&lt;ss:Workbook xmlns:ss="urn:schemas-microsoft-com:office:spreadsheet"&gt;
    &lt;ss:Worksheet ss:
     Name="Sheet1"&gt;
        &lt;ss:Table&gt;
        &lt;/ss:Table&gt;
    &lt;/ss:Worksheet&gt;
&lt;/ss:Workbook&gt;
</pre>
</div>
</div>
</div>
&nbsp;
<div id="s4" class="ArticleTypeTitle">4. Add the Header Row</div>
<div class="ArticleNormalPara">The first row in the table you want to generate has "First Name", "Last Name", and "Phone Number" in the three columns. Let’s add a &lt;Row&gt; tag as well as three &lt;Cell&gt; tags. The actual content of the cell is contained within a &lt;Data&gt; tag, so let’s add that as well. The file now looks like <strong>Figure 2</strong>.</div>
<div class="figmargin">
<div class="MTPS_CollapsibleRegion">
<div class="CollapseRegionLink"><img class="LibC_o" src="http://i.msdn.microsoft.com/Global/Images/clear.gif" alt="" />  Figure 2 XML Worksheet Takes Form</div>
<div class="MTPS_CollapsibleSection">
<div id="ctl00_MTContentSelector1_mainContentContainer_ctl27_ctl00_ctl00_" class="libCScode">
<div class="CodeSnippetTitleBar"></div>
<div dir="ltr">
<pre class="libCScode">&lt;?xml version="1.0"?&gt;
&lt;ss:Workbook xmlns:ss="urn:schemas-microsoft-com:office:spreadsheet"&gt;
    &lt;ss:Worksheet ss:Name="Sheet1"&gt;
        &lt;ss:Table&gt;
            &lt;ss:Row&gt;
                &lt;ss:Cell&gt;
                    &lt;ss:Data ss:Type="String"&gt;First Name&lt;/ss:Data&gt;
                &lt;/ss:Cell&gt;
                &lt;ss:Cell&gt;
                    &lt;ss:Data ss:Type="String"&gt;Last Name&lt;/ss:Data&gt;
                &lt;/ss:Cell&gt;
                &lt;ss:Cell&gt;&lt;ss:Data ss:Type="String"&gt;Phone Number&lt;/ss:Data&gt;
                &lt;/ss:Cell&gt;
            &lt;/ss:Row&gt;
        &lt;/ss:Table&gt;
    &lt;/ss:Worksheet&gt;
&lt;/ss:Workbook&gt;

</pre>
</div>
</div>
</div>
</div>
</div>
<div class="ArticleNormalPara">You now have a template for the table that you can open directly in Excel. It will look like <strong>Figure 3</strong>. Not too exciting, but it’s a start.</div>
<div class="ArticleImageSpacer">

<img src="http://i.technet.microsoft.com/cc161037.fig03%28en-us%29.gif" alt="" />
<div class="ArticleImageCaptionText">Figure 3<strong> Rudimentary Worksheet </strong></div>
</div>
&nbsp;
<div id="s5" class="ArticleTypeTitle">5. Adjust the Column Widths</div>
<div class="ArticleNormalPara">Notice that the widths of the columns are too narrow for the content. Let’s add some XML to the file to specify the width you want for the columns. The resulting code is shown in <strong>Figure 4</strong>.</div>
<div class="figmargin">
<div class="MTPS_CollapsibleRegion">
<div class="CollapseRegionLink"><img class="LibC_o" src="" alt="" />  Figure 4 Resizing the Columns</div>
<div class="MTPS_CollapsibleSection">
<div id="ctl00_MTContentSelector1_mainContentContainer_ctl28_ctl00_ctl00_" class="libCScode">
<div class="CodeSnippetTitleBar"></div>
<div dir="ltr">
<pre class="libCScode">&lt;?xml version="1.0"?&gt;
&lt;ss:Workbook xmlns:ss="urn:schemas-microsoft-com:office:spreadsheet"&gt;
    &lt;ss:Worksheet ss:Name="Sheet1"&gt;
        &lt;ss:Table&gt;
            &lt;ss:Row&gt;
                &lt;ss:Cell&gt;
                    &lt;ss:Data ss:Type="String"&gt;First Name&lt;/ss:Data&gt;
                &lt;/ss:Cell&gt;
                &lt;ss:Cell&gt;
                    &lt;ss:Data ss:Type="String"&gt;Last Name&lt;/ss:Data&gt;
                &lt;/ss:Cell&gt;
                &lt;ss:Cell&gt;&lt;ss:Data ss:Type="String"&gt;Phone Number&lt;/ss:Data&gt;
                &lt;/ss:Cell&gt;
            &lt;/ss:Row&gt;
        &lt;/ss:Table&gt;
    &lt;/ss:Worksheet&gt;
&lt;/ss:Workbook&gt;

</pre>
</div>
</div>
</div>
</div>
</div>
<div class="ArticleNormalPara">Now open the file again in Excel. Notice that the columns are wider and that the text now fits (see <strong>Figure 5</strong>). There is another attribute you can set on the column element that tells it to use autofit for the widths. This only works for numbers and dates though. Since your cells are strings, you need to explicitly set the width.</div>
<div class="ArticleImageSpacer">

<img src="" alt="" />
<div class="ArticleImageCaptionText">Figure 5<strong> Resized Cells </strong></div>
</div>
&nbsp;
<div id="s6" class="ArticleTypeTitle">6. Add the Remaining Data</div>
<div class="ArticleNormalPara">Now add those additional rows of data. This should be pretty easy. Just select that first "row" element and copy it. Then paste it five more times so you have six total rows. Now go through and update the values of the rows. If you are familiar with Extensible Stylesheet Language Transform (XSLT), you’ll see how you could easily generate an XSLT that could be applied to a DataSet to transform it into SpreadsheetML. Just repeat the Row tag for each row in your DataSet and add the values in each cell’s Data tag. After applying all the data, your XML should look like <strong>Figure 6</strong>, which has been abbreviated for space. <strong>Figure 7</strong> shows the full table in Excel.</div>
<div class="figmargin">
<div class="MTPS_CollapsibleRegion">
<div class="CollapseRegionLink"><img class="LibC_o" src="" alt="" />  Figure 6 XML Table with all Data</div>
<div class="MTPS_CollapsibleSection">
<div id="ctl00_MTContentSelector1_mainContentContainer_ctl29_ctl00_ctl00_" class="libCScode">
<div class="CodeSnippetTitleBar"></div>
<div dir="ltr">
<pre class="libCScode">&lt;?xml version="1.0"?&gt;
&lt;ss:Workbook xmlns:ss="urn:schemas-microsoft-com:office:spreadsheet"&gt;
    &lt;ss:Worksheet ss:Name="Sheet1"&gt;
        &lt;ss:Table&gt;
            &lt;ss:Column ss:Width="80"/&gt;
            &lt;ss:Column ss:Width="80"/&gt;
            &lt;ss:Column ss:Width="80"/&gt;
            &lt;ss:Row&gt;
                &lt;ss:Cell&gt;
                   &lt;ss:Data ss:Type="String"&gt;First Name&lt;/ss:Data&gt;
                &lt;/ss:Cell&gt;
                &lt;ss:Cell&gt;
                   &lt;ss:Data ss:Type="String"&gt;Last Name&lt;/ss:Data&gt;
                &lt;/ss:Cell&gt;
                &lt;ss:Cell&gt;
                   &lt;ss:Data ss:Type="String"&gt;Phone Number&lt;/ss:Data&gt;
                &lt;/ss:Cell&gt;
            &lt;/ss:Row&gt;
            &lt;ss:Row&gt;
                &lt;ss:Cell&gt;
                   &lt;ss:Data ss:Type="String"&gt;Nancy&lt;/ss:Data&gt;
                &lt;/ss:Cell&gt;
                &lt;ss:Cell&gt;
                   &lt;ss:Data ss:Type="String"&gt;Davolio&lt;/ss:Data&gt;
                &lt;/ss:Cell&gt;
                &lt;ss:Cell&gt;
                   &lt;ss:Data ss:Type="String"&gt;(206)555 9857&lt;/ss:Data&gt;
                &lt;/ss:Cell&gt;
            &lt;/ss:Row&gt;
            &lt;ss:Row&gt;
            ...
            &lt;/ss:Row&gt;
        &lt;/ss:Table&gt;
    &lt;/ss:Worksheet&gt;
&lt;/ss:Workbook&gt;

</pre>
</div>
</div>
</div>
</div>
</div>
<div class="ArticleImageSpacer">

<img src="" alt="" />
<div class="ArticleImageCaptionText">Figure 7<strong> Worksheet with Data </strong></div>
</div>
&nbsp;
<div id="s7" class="ArticleTypeTitle">7. Add Header Formatting</div>
<div class="ArticleNormalPara">

As you can see, the first row does not look like a column header, so let’s format it with bold text so that it’s clearly the header. All you need to do is generate a style that has bold text, and then reference that style with the first row. First, add the following XML in front of the Worksheet tag:
<div id="ctl00_MTContentSelector1_mainContentContainer_ctl16_" class="libCScode">
<div class="CodeSnippetTitleBar"></div>
<div dir="ltr">
<pre class="libCScode">&lt;ss:Styles&gt;
    &lt;ss:Style ss:ID="1"&gt;
        &lt;ss:Font ss:Bold="1"/&gt;
    &lt;/ss:Style&gt;
&lt;/ss:Styles&gt;
</pre>
</div>
</div>
This creates a style whose ID is "1" and has bold applied to it. Next, update the first row element to reference StyleID 1. The row code should now look like this:
<div id="ctl00_MTContentSelector1_mainContentContainer_ctl17_" class="libCScode">
<div class="CodeSnippetTitleBar"></div>
<div dir="ltr">
<pre class="libCScode">&lt;ss:Row ss:SyleID="1"&gt;
</pre>
</div>
</div>
Your XML should now look like <strong>Figure 8</strong>, and <strong>Figure 9</strong> shows how it looks in Excel.

</div>
<div class="figmargin">
<div class="MTPS_CollapsibleRegion">
<div class="CollapseRegionLink"><img class="LibC_o" src="" alt="" />  Figure 8 Bolding the Header Row</div>
<div class="MTPS_CollapsibleSection">
<div id="ctl00_MTContentSelector1_mainContentContainer_ctl32_ctl00_ctl00_" class="libCScode">
<div class="CodeSnippetTitleBar"></div>
<div dir="ltr">
<pre class="libCScode">&lt;?xml version="1.0"?&gt;
&lt;ss:Workbook xmlns:ss="urn:schemas-microsoft-com:office:spreadsheet"&gt;
    &lt;ss:Styles&gt;
        &lt;ss:Style ss:ID="1"&gt;
            &lt;ss:Font ss:Bold="1"/&gt;
        &lt;/ss:Style&gt;
    &lt;/ss:Styles&gt;
    &lt;ss:Worksheet ss:Name="Sheet1"&gt;
        &lt;ss:Table&gt;
            &lt;ss:Column ss:Width="80"/&gt;
            &lt;ss:Column ss:Width="80"/&gt;
            &lt;ss:Column ss:Width="80"/&gt;
            &lt;ss:Row ss:StyleID="1"&gt;
                &lt;ss:Cell&gt;
                   &lt;ss:Data ss:Type="String"&gt;First Name&lt;/ss:Data&gt;
                &lt;/ss:Cell&gt;
                &lt;ss:Cell&gt;
                   &lt;ss:Data ss:Type="String"&gt;Last Name&lt;/ss:Data&gt;
                &lt;/ss:Cell&gt;
                &lt;ss:Cell&gt;
                   &lt;ss:Data ss:Type="String"&gt;Phone Number&lt;/ss:Data&gt;
                &lt;/ss:Cell&gt;
            &lt;/ss:Row&gt;
            &lt;ss:Row&gt;
                &lt;ss:Cell&gt;
                   &lt;ss:Data ss:Type="String"&gt;Nancy&lt;/ss:Data&gt;
                &lt;/ss:Cell&gt;
                &lt;ss:Cell&gt;
                   &lt;ss:Data ss:Type="String"&gt;Davolio&lt;/ss:Data&gt;
                &lt;/ss:Cell&gt;
                &lt;ss:Cell&gt;
                   &lt;ss:Data ss:Type="String"&gt;(206)555-9857&lt;/ss:Data&gt;
                &lt;/ss:Cell&gt;
            &lt;/ss:Row&gt;
            ...
            &lt;/ss:Row&gt;
        &lt;/ss:Table&gt;
    &lt;/ss:Worksheet&gt;
&lt;/ss:Workbook&gt;

</pre>
</div>
</div>
</div>
</div>
</div>
<div class="ArticleImageSpacer">

<img src="" alt="" />
<div class="ArticleImageCaptionText">Figure 9<strong> The Completed Worksheet </strong></div>
</div>
&nbsp;
<div id="s8" class="ArticleTypeTitle">Wrap-Up</div>
<div class="ArticleNormalPara">That was a pretty simple example, but it’s a good introduction if you’re new to Office XML (or even new to XML in general). The new XML formats for future versions of Excel will look different than what I’ve shown you with SpreadsheetML, but there will also be some similarities. It’s good to become familiar with the existing schemas, and I’ll start posting a lot more about the new schemas on my blog at <a href="http://blogs.msdn.com/brian_jones" target="_blank">blogs.msdn.com/brian_jones</a>.</div>
