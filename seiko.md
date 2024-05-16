Uts PA agun
Public Class FormLogin

    Private ReadOnly validUsername As String = "Kurniawan"
    Private ReadOnly validPassword As String = "220660221045"

    Private Sub btnLogin_Click(sender As Object, e As EventArgs) Handles btnLogin.Click
        If txtUsername.Text = validUsername AndAlso txtPassword.Text = validPassword Then
            Dim formPenjualan As New FormPenjualan(validUsername, validPassword)
            formPenjualan.Show()
            Me.Hide()
        Else
            MessageBox.Show("Username atau Password salah!", "Login Gagal", MessageBoxButtons.OK, MessageBoxIcon.Information)
        End If


    End Sub
End Class

Public Class FormPenjualan
    Private kasirNama As String
    Private kasirNPM As String

    Public Sub New(nama As String, npm As String)
        InitializeComponent()
        kasirNama = nama
        kasirNPM = npm
        lblNamaKasir.Text = "Nama: " & kasirNama
        lblNPMKasir.Text = "NPM: " & kasirNPM
    End Sub

    Private Sub btnHitung_Click(sender As Object, e As EventArgs) Handles btnHitung.Click
        Dim totalPembelian As Decimal
        If Decimal.TryParse(txtTotalPembelian.Text, totalPembelian) Then
            Dim diskon As Decimal = 0

            If totalPembelian > 1000000 Then
                diskon = 0.035D
            ElseIf totalPembelian > 500000 Then
                diskon = 0.015D
            ElseIf totalPembelian > 100000 Then
                diskon = 0.01D
            End If

            Dim totalDiskon As Decimal = totalPembelian * diskon
            Dim totalBayar As Decimal = totalPembelian - totalDiskon

            lblDiskon.Text = "Diskon: " & totalDiskon.ToString("C")
            lblTotalBayar.Text = "Total Bayar: " & totalBayar.ToString("N2")
        Else
            MessageBox.Show("Input tidak valid untuk total pembelian!", "Error", MessageBoxButtons.OK, MessageBoxIcon.Error)
        End If
    End Sub

    Private Sub btnBayar_Click(sender As Object, e As EventArgs) Handles btnBayar.Click
        Dim totalBayar As Decimal
        Dim totalBayarText As String = lblTotalBayar.Text.Replace("Total Bayar: ", "").Replace("Rp", "").Replace("$", "").Trim()

        If Decimal.TryParse(totalBayarText, totalBayar) Then
            Dim uangDibayarkan As Decimal
            If Decimal.TryParse(txtUangDibayarkan.Text, uangDibayarkan) Then
                If uangDibayarkan >= totalBayar Then
                    Dim kembalian As Decimal = uangDibayarkan - totalBayar
                    MessageBox.Show("Pembayaran berhasil! Kembalian: " & kembalian.ToString("C"), "Informasi", MessageBoxButtons.OK, MessageBoxIcon.Information)
                Else
                    MessageBox.Show("Uang yang dibayarkan kurang!", "Pembayaran Gagal", MessageBoxButtons.OK, MessageBoxIcon.Information)
                End If
            Else
                MessageBox.Show("Input tidak valid untuk uang yang dibayarkan!", "Error", MessageBoxButtons.OK, MessageBoxIcon.Error)
            End If
        Else
            MessageBox.Show("Total bayar tidak valid!", "Error", MessageBoxButtons.OK, MessageBoxIcon.Error)
        End If
    End Sub
End Class
