```VB
Public Shared Function API_VNP_DANHSACHNHAN_CCT(ByVal MaTinh As Integer, ByVal MaHuyen As Integer, ByVal KyChiTra As Integer, ByVal DSChuaChiTra As DataTable, Optional ByVal rt As ReturnType = ReturnType.DataTable) As DataSet
    'Return ExecuteDatasetWithTimeoutList(ConnectionString, "API_VNP_DANHSACHNHAN_CCT", 360, MaTinh, MaHuyen, KyChiTra, DSChuaChiTra)
    Dim startTime As Date = Now
    Dim ds As DataSet = New DataSet()
    Dim connection As New SqlConnection(ConnectionString)
    Try
        Dim command As New SqlCommand("API_VNP_DANHSACHNHAN_CCT", connection)
        command.CommandType = CommandType.StoredProcedure

        connection.Open()
        command.Parameters.AddWithValue("@MaTinh", MaTinh)
        command.Parameters.AddWithValue("@MaHuyen", MaHuyen)
        command.Parameters.AddWithValue("@KyChiTra", KyChiTra)
        command.Parameters.AddWithValue("@IN_DSChuaChiTra", DSChuaChiTra)
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
    Return ds
End Function
```
