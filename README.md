using MySql.Data.MySqlClient;
using System;
using System.Drawing;
using System.Windows.Forms;

namespace WindowsFormsApp
{
    public partial class AnaForm : Form
    {
        public AnaForm()
        {
            InitializeComponent();
            btnGiris.Click += new EventHandler(button1_Click);
        }

        private void AnaMenü_Load(object sender, EventArgs e)
        {
           
        }

        private void button1_Click(object sender, EventArgs e)
        {
            string searchValue = textBox2.Text; 
            if (string.IsNullOrEmpty(searchValue))
            {
                MessageBox.Show("Lütfen aramak istediğiniz ID'yi girin.");
                return;
            }

            try
            {
                string connectionstring = "server=localhost;uid=EchomeraN;pwd=22424423;database=hastaneyonetim";
                using (MySqlConnection connection = new MySqlConnection(connectionstring))
                {
                    connection.Open();
                    string sql = "SELECT * FROM doktorlar WHERE ID = @searchValue"; 
                    MySqlCommand cmd = new MySqlCommand(sql, connection);
                    cmd.Parameters.AddWithValue("@searchValue", searchValue); 

                    using (MySqlDataReader reader = cmd.ExecuteReader())
                    {
                        if (reader.HasRows)
                        {
                            while (reader.Read())
                            {
                                if (reader["ID"].ToString() == "123123")
                                {
                                    MessageBox.Show("Eşleşen Kayıt Bulundu:\nID: " + reader["ID"] + "\nİsim: " + reader["İsim"] + "\nUzmanlık: " + reader["Uzmanlik"]);
                                }
                                else
                                {
                                    MessageBox.Show("Eşleşen kayıt bulunamadı.");
                                }
                            }
                        }
                        else
                        {
                            MessageBox.Show("Eşleşen kayıt bulunamadı.");
                        }
                    }
                }
            }
            catch (MySqlException ex)
            {
                MessageBox.Show(ex.ToString());
            }
        }

      
    }
}
