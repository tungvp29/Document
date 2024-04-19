## Function to send list items (DataTable) to Store procedure
```VB 
Public Overrides Sub TN_KPI_DashboardInfo_AddUpdate(PortalConnect As String, items As DataTable)
    'Return ExecuteDatasetWithTimeoutList(ConnectionString, "API_VNP_DANHSACHNHAN_CCT", 360, MaTinh, MaHuyen, KyChiTra, DSChuaChiTra)
    Dim startTime As Date = Now
    Dim ds As DataSet = New DataSet()
    Dim connection As New SqlConnection(New flConnect().KpiConnect(PortalConnect))
    Try
        Dim command As New SqlCommand("TN_KPI_DashboardInfo_AddUpdate", connection)
        command.CommandType = CommandType.StoredProcedure

        connection.Open()
        command.Parameters.AddWithValue("@DashboardInfos", items)
        command.CommandTimeout = 360
        'command.ExecuteNonQuery()
        Dim adapter As New SqlDataAdapter(command)
        adapter.Fill(ds)
    Catch ex As Exception
        Throw ex
    Finally
        If connection IsNot Nothing Then
            If connection.State = ConnectionState.Open Then
                connection.Close()
            End If
        End If
    End Try
    Dim endTime As Date = Now
End Sub
```

## Convert from ArrayList to DataTable
```VB
Function ConvertArrayListToDataTable(ByVal arraylist As ArrayList) As DataTable
    Dim dt As DataTable = New DataTable()

    If arraylist.Count <= 0 Then
        Return dt
    End If

    Dim propertiesinfo As PropertyInfo() = arraylist(0).GetType().GetProperties()

    For Each pf As PropertyInfo In propertiesinfo
        Dim dc As DataColumn = New DataColumn(pf.Name)
        dc.DataType = pf.PropertyType
        dt.Columns.Add(dc)
    Next

    For Each ar As Object In arraylist
        Dim dr As DataRow = dt.NewRow
        Dim pf As PropertyInfo() = ar.GetType().GetProperties()

        For Each prop As PropertyInfo In pf
            dr(prop.Name) = prop.GetValue(ar, Nothing)
        Next
        dt.Rows.Add(dr)
    Next
    Return dt
End Function
```

## Function called in presentations:
```VB
controller.TN_KPI_DashboardInfo_AddUpdate(_tabName, ConvertArrayListToDataTable(DashboardInfos))
```
