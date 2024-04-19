### Function to send list items (DataTable) to Store procedure
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

### Convert from ArrayList to DataTable
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

### Function called in presentations:
```VB
controller.TN_KPI_DashboardInfo_AddUpdate(_tabName, ConvertArrayListToDataTable(DashboardInfos))
```

### SQL Upsert store procedure from list items
```SQL
ALTER PROCEDURE [dbo].[TN_KPI_DashboardInfo_AddUpdate]
(      
	 @DashboardInfos List_TN_KPI_DashboardInfo READONLY
)
AS
BEGIN
	DECLARE @OutputTbl TABLE (Id INT)


	MERGE TN_KPI_DashboardInfo AS t  
	USING (  
		SELECT * FROM @DashboardInfos  
	) AS s ON t.Username = s.Username and t.YearOfScore = s.YearOfScore
	WHEN MATCHED THEN  
		UPDATE SET  
			t.STT = s.STT
			,t.Username = s.Username
			,t.UserID = s.UserID
			,t.DisplayName = s.DisplayName
			,t.Total = s.Total
			,t.Provinder_ID = s.Provinder_ID
			,t.Provinder_Name = s.Provinder_Name
			,t.Provinder_Code = s.Provinder_Code
			,t.Ranks = s.Ranks
			,t.Score = s.Score
			,t.Position_Name = s.Position_Name
			,t.VaiTro = s.VaiTro
			,t.CreatedOnDate = s.CreatedOnDate
			,t.IsBoss = s.IsBoss
			,t.DocumentMustDone = s.DocumentMustDone
			,t.DocumentOTDone = s.DocumentOTDone
			,t.DocumentOTNotDone = s.DocumentOTNotDone
			,t.DocumentOTGiaHan = s.DocumentOTGiaHan
			,t.DocumentOTReturn = s.DocumentOTReturn
			,t.DocumentLienDoi = s.DocumentLienDoi
			,t.Modified = s.Modified
			,t.YearOfScore = s.YearOfScore
	WHEN NOT MATCHED THEN  
		INSERT (  
			STT, Username, UserID, DisplayName, Total, 
			Provinder_ID, Provinder_Name, Provinder_Code, 
			Ranks, Score, Position_Name, VaiTro, CreatedOnDate, 
			IsBoss, 
			DocumentMustDone, 
			DocumentOTDone, 
			DocumentOTNotDone, 
			DocumentOTGiaHan, 
			DocumentOTReturn, 
			DocumentLienDoi, 
			Modified, 
			YearOfScore
		)  
		VALUES (  
			s.STT, 
			s.Username, 
			s.UserID, 
			s.DisplayName, 
			s.Total, 
			s.Provinder_ID, 
			s.Provinder_Name, 
			s.Provinder_Code, 
			s.Ranks, 
			s.Score, 
			s.Position_Name, 
			s.VaiTro, 
			s.CreatedOnDate, 
			s.IsBoss, 
			s.DocumentMustDone, 
			s.DocumentOTDone, 
			s.DocumentOTNotDone, 
			s.DocumentOTGiaHan, 
			s.DocumentOTReturn, 
			s.DocumentLienDoi, 
			s.Modified, 
			s.YearOfScore
		);  

	SELECT COUNT(Id) FROM @OutputTbl
END

```
