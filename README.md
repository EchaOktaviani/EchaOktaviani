package main

import (
	"database/sql"
	"fmt"

	_ "github.com/go-sql-driver/mysql"
)

// Struktur data mahasiswa
type student struct {
	nama     string
	nim      string
	jurusan  string
	fakultas string
}

// Fungsi koneksi ke database
func connect() (*sql.DB, error) {
	db, err := sql.Open("mysql", "root:@tcp(127.0.0.1:3306)/db_mhs")
	if err != nil {
		return nil, err
	}
	return db, nil
}

// Fungsi untuk menjalankan query SQL dan mengambil data
func sqlQuery() {
	db, err := connect()
	if err != nil {
		fmt.Println(err.Error())
		return
	}
	defer db.Close()

	rows, err := db.Query("select nama, nim, jurusan, fakultas from mahasiswa")
	if err != nil {
		fmt.Println(err.Error())
		return
	}
	defer rows.Close()

	var result []student

	for rows.Next() {
		var each student
		err := rows.Scan(&each.nama, &each.nim, &each.jurusan, &each.fakultas)
		if err != nil {
			fmt.Println(err.Error())
			return
		}
		result = append(result, each)
	}
	if rows.Err() != nil {
		fmt.Println(rows.Err().Error())
		return
	}

	for _, each := range result {
		fmt.Printf("Nama: %s, NIM: %s, Jurusan: %s, Fakultas: %s\n", each.nama, each.nim, each.jurusan, each.fakultas)
	}
}

func main() {
	sqlQuery()
}
