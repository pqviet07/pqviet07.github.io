# Git Note

- **Pull tất cả branch ở remote về local**

    ```bash
    git branch -r | grep -v '\->' | while read remote; do git branch --track "${remote#origin/}" "$remote"; done
    git fetch --all
    git pull --all
    ```

- **Pull 1 branch cụ thể ở remote về local**

    ```bash
    git switch <branch_name>
    ```

- **Delete 1 branch ở remote**

    ```bash
    git push -d origin <branch_name>
    ```

- **Nếu git bắt buộc retype password mỗi lần pull thì có 2 cách giải quyết**
    1. Sử dụng SSH
    2. Dùng lệnh:

    ```bash
    git config --global credential.helper 'cache --timeout 86400000'
    git config credential.helper store
    ```

- **Xử lí khi muốn thay đổi code đã commit**

    ```bash
    1. git rebase --interactive 'bbc643cd^' #bbc643cd is the commit you want to modify
    2. Change "pick" into "edit"
    3. git commit --all --amend
    4. git rebase --continue
    5. git push --force
    ```

- Trong trường hợp làm mất file do lỡ **git stash pop** thì có thể recovery bằng cách

    ```bash
    git fsck --no-reflog | awk '/dangling commit/ {print $3}'
    ```

- **Merge development vào 1 nhánh nào đó**

    ```bash
    #Approach 1:
    git fetch origin aRemoteBranch
    git merge origin/aRemoteBranch
    # Trick: alias combine = !git fetch origin ${1} && git merge origin/${1}

    #Approach 2:
    git pull origin aRemoveBranch #(include two steps at approach 1)

    #Approach 3:
    git fetch origin
    git rebase origin/master
    ```

- Bên dưới **git pull** bao gồm 2 bước

    ```bash
    git fetch origin
    git merge origin/<branch_name>
    ```

**Note**

- `master` is your local branch.
- `origin master` is the branch `master` on the remote repository named `origin`.
- `origin/master` is your local copy of `origin master`.

Hệ thống git có **3 objects** cơ bản: **blob, tree, commit.** Mọi thứ phức tạp khác chỉ tham chiếu vào các object này

![Git%20Note%208dce784cddd94ef7a8a07721985efbcc/commits.png](Git%20Note%208dce784cddd94ef7a8a07721985efbcc/commits.png)

![Git%20Note%208dce784cddd94ef7a8a07721985efbcc/httpatomoreillycomsourceoreillyimages1703687.png](Git%20Note%208dce784cddd94ef7a8a07721985efbcc/httpatomoreillycomsourceoreillyimages1703687.png)

- Một số **merge strategy**
    1. **Fast-forward**: ví dụ khi nhánh master ko có gì thay đổi thì git merge master sẽ là fast-forward → không tạo ra merge commit
    2. **Recursive**: Ngược với fast-forward → có tạo ra merge commit
    3. **Octopus:** khi muốn merge nhiều branch cùng lúc. 

        ```bash
        git merge featureA featureB featureC
        ```

    4. **Subtree:** sử dụng khi repo lồng nhau *(chưa hiểu strategy này, rảnh tìm hiểu lại)*

    [Vài link hay ho về git](https://www.notion.so/777fcd4307b441eda6bd543415ad9466)