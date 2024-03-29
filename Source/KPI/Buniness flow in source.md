**1. Update static dashboard info flow**

InvokeIntegrate.vb <br>
Method **```UpdateDashboard```**<br>
=> get current detail data from store procedure **TN_KPI_Dashboard_By_ProvinderID**<br>
=> calculate in **flUtil.GetDocuments()**<br>
=> upsert calculated table via store procedure **TN_KPI_DashboardInfo_AddUpdate**<br>
=> write log KPI_SCHEDULE_LOG
