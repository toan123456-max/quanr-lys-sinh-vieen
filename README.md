# quanr-lys-sinh-vieen

Public Class Form1
    Private Sub Form1_Load(sender As Object, e As EventArgs) Handles MyBase.Load
        Dim connectionString As String = "Data Source=(LocalDB)\MSSQLLocalDB;AttachDbFilename=C:\YourPath\YourDB.mdf;Integrated Security=True;"
        Public Sub LoadStudents()
        Try
            conn.Open()
            Dim query As String = "SELECT * FROM Students"
            Dim da As New System.Data.SqlClient.SqlDataAdapter(query, conn)
            Dim dt As New DataTable()
            da.Fill(dt)
            dgvStudents.DataSource = dt

        Catch ex As Exception
            MessageBox.Show("Lỗi đăng nhập : " & ex.Message)
        Finally
            conn.Close()
        End Try
    End Sub

    Private Sub btnSave_Click(sender As Object, e As EventArgs) Handles btnSave.Click

        '
        Dim mssv As String = txtMSSV.Text.Trim()
        Dim fullName As String = txtName.Text.Trim()
        Dim studentClass As String = txtClass.Text.Trim()

        '
        If String.IsNullOrEmpty(mssv) OrElse String.IsNullOrEmpty(fullName) Then
            MessageBox.Show("Vui lòng nhập đầy đủ Mã Sinh Viên và Họ Tên.", "Thiếu Thông Tin", MessageBoxButtons.OK, MessageBoxIcon.Warning)
            Return
        End If


        Dim query As String = "INSERT INTO Students (MSSV, FullName, Class) VALUES (@mssv, @name, @class)"

        Using cmd As New System.Data.SqlClient.SqlCommand(query, conn)

            cmd.Parameters.AddWithValue("@mssv", mssv)
            cmd.Parameters.AddWithValue("@name", fullName)
            cmd.Parameters.AddWithValue("@class", studentClass)

            Try
                conn.Open()

                Dim rowsAffected As Integer = cmd.ExecuteNonQuery()

                If rowsAffected > 0 Then
                    MessageBox.Show("Thêm sinh viên thành công!", "Thành Công", MessageBoxButtons.OK, MessageBoxIcon.Information)


                    txtMSSV.Clear()
                    txtName.Clear()
                    txtClass.Clear()
                    txtMSSV.Focus()
                Else
                    MessageBox.Show("Không thể thêm sinh viên. Vui lòng kiểm tra lại.", "Lỗi", MessageBoxButtons.OK, MessageBoxIcon.Error)
                End If

            Catch ex As Exception

                MessageBox.Show("Lỗi Database: " & ex.Message, "Lỗi Kết Nối", MessageBoxButtons.OK, MessageBoxIcon.Error)

            Finally

                If conn.State = ConnectionState.Open Then
                    conn.Close()
                End If
        End Using


        Dim scoreForm As New Form2()


        scoreForm.ShowDialog()

    End Sub
    Private Sub btnExit_Click(sender As Object, e As EventArgs) Handles btnExit.Click
        Me.Close()
    End Sub
End Class
