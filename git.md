# Push dengan repository yang sudah ada

1. Cek url existing
    ```
    git remote -v
    ```
2. Tambahkan url remote
    ```
    git remote add origin https://github.com/triyasmkom/tutorial_repo.git

    ```
3. Update repository dari remote
    ```
    git fetch
    ```

4. Ambil repository dari remote
    ```
    git pull origin master
    ```
5. Masukkan semua file/folder yang ingin kita commit
    ```
    git add .
    ```
6. Commit semua perubahan
    ```
    git commit -m "tutoarial git"
    ```
7. Push ke repository remote
    ```
    git push origin master
    ```

# Dasar-dasar comment line git
1. membuat repository
    ```
    git init
    ```
2. melihat status perubahan file
    ```
    git status
    ```
3. menambahkan commit
    ```
    git add <nama file/folder>
    ```
4. melakukan commit
    ```
    git commit -m "message commit"
    ```
5. pindah/membuat branch
    ```
    git checkout <nama_branch>
    ```
6. melihat perbedaan perubahan
    ```
    git diff
    ```
7. menghubungkan repository local dengan server
    ```
    git remote add origin <alamat server>
    ```
8. membuat branch baru
    ```
    git checkout -b <nama_branch>
    ```
9. pindah branch 
    ```
    git checkout <branch yg dituju>
    ```
10. menghapus branch
    ```
    git branch -d feature_a
    git branch --delete feature_a
    ```
11. menghapus branch secara paksa
    ```
    git branch -D feature_a

    git branch --delete --force feature_a

    *-D = --delete --force
    ```
12. hapus branch remote
    ```
    git push -d <remote_name> <branch_name>
    git push -d origin example
    ```
13. update dan merging
    ```
    git pull

    git merge <nama_branch>
    ```


# Menyimpan sementara di local sebelum melakukan pull dari remote
1. Cek status perubahan
    ```
    git status
    ```
2. mengabaikan perubahan
    ```
    git stash
    ```
3. pull dari remote
    ```
    git pull
    ```
4. mengembalikan perubahan
    ```
    git stash pop
    ```
5. melihat daftar stash
    ```
    git stash list
    ```
6. menghapus stash
    ```
    git stash clear
    ```
