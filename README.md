# Tesla-rent

using Microsoft.Data.Sqlite;
using System;

class Program
{
    static void Main(string[] args)
    {
        // Connection string for SQLite
        string connectionString = "Data Source=tesla_rent.db";

        try
        {
            // Create and open the SQLite connection
            using (var connection = new SqliteConnection(connectionString))
            {
                connection.Open();

                // Create table if it doesn't exist
                var createTableCmd = connection.CreateCommand();
                createTableCmd.CommandText = @"
                    CREATE TABLE IF NOT EXISTS Teslas (
                        Id INTEGER PRIMARY KEY AUTOINCREMENT,
                        Model TEXT NOT NULL,
                        HourlyRate DECIMAL NOT NULL,
                        KilometerRate DECIMAL NOT NULL
                    );
                ";
				createTableCmd.ExecuteNonQuery();
				Console.WriteLine("Table created or already exists.");
				
				//Promt user for tesla model
                Console.WriteLine("Please enter Tesla model");
                string teslaModel = Console.ReadLine();
                
                //Promt user for tesla horlyrate
                Console.WriteLine("Please enter Tesla hourlyrate");
                string teslaHourlyrate = Console.ReadLine();
				
				//Promt user for tesla kilometerarte
                Console.WriteLine("Please enter Tesla kilometerrate");
                double teslaKilometerrate = Convert.ToDouble(Console.ReadLine());
				
				// Insert data in SQL lite database
                var insertCmd = connection.CreateCommand();
                insertCmd.CommandText = "INSERT INTO Teslas(Name, Size, Price) VALUES (@name, @size, @price)";
                insertCmd.Parameters.AddWithValue("@model", teslaModel);
                insertCmd.Parameters.AddWithValue("@hourlyrate", teslaHourlyrate);
                insertCmd.Parameters.AddWithValue("@kilometerrate", teslaKilometerrate);
				
				 insertCmd.ExecuteNonQuery();
                Console.WriteLine("TESLA IS ADDED TO DATABASE");
				
				// Query to display tesla database elements
                var selectCmd = connection.CreateCommand();
                selectCmd.CommandText = "SELECT * FROM TESLAS";
				
				using (var reader = selectCmd.ExecuteReader())
                {
                    Console.WriteLine("Tesla List:");
                    while(reader.Read())
					{
                        Console.WriteLine($"Id: {reader["Id"]}, Model: {reader["Model"]}, Horlyrate: {reader["Horlyrate"]}, Kilometerrate: {reader["Kilometerrate"]}");
                    }
                }
            }
        }
        catch (Exception ex)
        {
            Console.WriteLine("Test");
        }
    }
}
