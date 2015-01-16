# Telerik-report-for-beginners-

In this artcle we discuss how to create and programmatically bind telerik report.

## Introduction
This article will help you to create reports using Telerik Reports. Here I am giving a step by step idea to create and programmatically bind telerik report to web page. Telerik has powerful capabilities to access and analyse various sources of data for its reports.

## Background
As you already know telerik is not free reporting tool. But you can download its trail version to use. After install the telerik reporting to visual studio following steps may helpful you as a beginner. 


#### Step 1
As first step, open visual studio with Asp .net web application. Then add a new class library for reporting purpose. To this library, add blank telerik report as shown in the image. Report version depend on your installatio. If you select Telerik report import wizard you may follow the wizard to make your report. But in this article we only discuss how to data bind programmatically since there are lot of articles and video tutorials for the report wizard.

![alt tag](http://www.codeproject.com/KB/aspnet/865568/Add_report.jpg)



#### Step 2
Open the report. By default it will open report designer. Press “F7” in the report designer to move code behind as you already know. Following straightforward code will take value from data base to report. Then you can directly bind “sqlDataAdapter1” to data sources using “this.DataSource” as shown in below.

```
using System.Data.SqlClient;

namespace TelerikReports
{
    using System;
    using System.ComponentModel;
    using System.Drawing;
    using System.Windows.Forms;
    using Telerik.Reporting;
    using Telerik.Reporting.Drawing;

    /// <summary>
    /// Summary description for SampleReport.
    /// </summary>
    public partial class SampleReport : Telerik.Reporting.Report
    {
        public SampleReport()
        {
           
            InitializeComponent();

            int Id = 7;

            SqlConnection sqlConnection1 = new SqlConnection();

            SqlCommand sqlCommand1 = new SqlCommand();

            SqlDataAdapter sqlDataAdapter1 = new SqlDataAdapter();

            sqlConnection1.ConnectionString = "Data Source=(local);Initial Catalog=ZssDcposDat;" +
                                              "Integrated Security=True;MultipleActiveResultSets=True";

            sqlCommand1.CommandText = "SELECT VoucherNo,ActivateOn,BatchId," +
                                      " VoucherValue FROM VoucherBatchDetails where BatchId =" + Id;

            sqlCommand1.Connection = sqlConnection1;

            sqlDataAdapter1.SelectCommand = sqlCommand1;

            this.DataSource = sqlDataAdapter1;
        }
    }
}

```

You can use any data receiving method from data base instead of ADO.Net such as entity frame work. following code is an example for entity frame work. As shown below, you can bind any modified data list for your report data source.
This sample code only for who use entity framework for their projects. Otherwise you can use above ADO.net code for receive data from data base.

```
using System.Collections.Generic;
using System.Linq;
using Report.Data;
using Report.Domain.Models;
using Report.Infrastructure;
using Report.Repository.Helpers;

namespace TelarikWebApp
{
    using System;
    using System.ComponentModel;
    using System.Drawing;
    using System.Windows.Forms;
    using Telerik.Reporting;
    using Telerik.Reporting.Drawing;

    public partial class SampleReport: Telerik.Reporting.Report
    {
        IZbsDcposUow Uow = new ZbsDcposUow(new RepositoryProvider(new RepositoryFactories()));

        public SampleReport()
        {
           
            InitializeComponent();

            // Load data from data base

            List<DataList> ReportData= Uow.VoucherDetailRepository.GetAllDistricts();

             var apiOutput = new
            {
                Rpt= ReportData.Select(a => new
                {
                    BatchId= a.BatchId,
                    VoucherNo= a.VoucherNo,
                    VoucherValue= a.VoucherValue == null ? 0 : VoucherValue,
                    ActivateOn = a.ActivateOn

                })
            };

            this.DataSource = apiOutput.Rpt;

        }
    }
}
```


### Step 3
Up to now you successfully complete data receiving part to your report. Now you have to bind these data to your report in proper way. Drag and drop text box to your report viewer. Type any name which you want to display. Eg. First test box name as “Bath Id”. Then add another text box below it. Then it rename as “=Fields.BatchId”. This text box will bind the data list’s data which related to Batch ids. “=Fields.” part is mandatory for all data bind from data base. “BatchId” is name of our selected data list field name. Bind all other fields to report as shown below.

![alt tag](http://www.codeproject.com/KB/aspnet/865568/Design___report.png)


### Step 4
Now you successfully create a report. You just need to bind this report to your web page. So add web form and drag and drop report viewer from tool box to this page as shown in below. In this step required code for the web config file will be automatically added.

![alt tag](http://www.codeproject.com/KB/aspnet/865568/add_report_Viwer.jpg)



### Step 5
Finally go to code behind in this web page and add following code. “SampleReport” in the code is the report name and you only need change this name to add different report for this web page.

```
using System;
using System.Collections.Generic;
using System.Linq;
using System.Web;
using System.Web.UI;
using System.Web.UI.WebControls;
using TelerikReports;

namespace TelerikWebAPP
{
    public partial class TelerikReportPage : System.Web.UI.Page
    {
        protected void Page_Load(object sender, EventArgs e)
        {
            var instanceReportSource = new Telerik.Reporting.InstanceReportSource();
            instanceReportSource.ReportDocument = new SampleReport();
            this.ReportViewer1.ReportSource = instanceReportSource;
        }
    }
}
```


### What is next ?
Now you have successfully created a simple report using telerik reporting. In next article we will discuss how to pass parameters to your report from web page and generate little more complex report using sub reports such as invoice. 







