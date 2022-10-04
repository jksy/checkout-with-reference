# GitHub Action checkout with reference parameter
GitHub Actions上でソースコードをcloneします。referenceを指定できるようにしています。

NFS等でリファレンス用のリポジトリを用意しておくと、ネットワーク転送量の低減や時間の短縮を期待できます。
リファレンスリポジトリについては [git](https://git-scm.com/docs/git-clone/ja) の `--reference` パラメータや、
https://swet.dena.com/entry/2021/07/12/120000 を参照してください。

# Usage

```yaml
- uses: jksy/checkout-with-reference@v1
  with:
    # https://github.com/jksy/checkout-with-reference.git の **jksy/checkout-with-reference** の部分です。
    # Default: ${{ github.repository }}
    github_repository: ''

    # Private repositoryの時、GitHubActionsより渡されるtokenを渡します
    # Default: ${{ github.token }}
    github_token: ''

    # Private repositoryの時、GitHubActionsより渡されるactorを渡します
    # Default: ${{ github.actor }}
    github_actor: ''

    # cloneするブランチ名を指定します。通常は指定なしです
    branch_name: ''

    # cloneするcommit shaを指定します。
    # Default: ${{ github.sha }}
    sha: ${{ github.sha }}

    # リファレンス先の clone 済みのディレクトリを指定します。
    # リファレンスリポジトリについては [git](https://git-scm.com/docs/git-clone/ja) の `--reference` パラメータを参照してください。
    reference_dir: '/mnt/shared_dir_like_as_nfs/repository.git'

    # clone時にLFSのデータをfetchするかどうかを指定します
    # clone後、git lfs pullしたほうが大体のケースで早いです
    fetch_lfs: false

    # checkout先のディレクトリ名を指定します
    checkout_dir: .
```

# Example

リファレンスリポジトリ(/mnt/shared_dir_like_as_nfs/repository.gitに存在する)付きで、カレントディレクトリにcheckoutします

```yaml
- uses: jksy/checkout-with-reference@v1
  with:
    reference_dir: '/mnt/shared_dir_like_as_nfs/repository.git'
    checkout_dir: .
```

プライベートリポジトリをcheckoutします

```yaml
- uses: jksy/checkout-with-reference@v1
  with:
    github_token: ${{ github.token }}
    github_actor: ${{ github.actor }}
    github_repository: ${{ github.repository }}
    reference_dir: '/mnt/shared_dir_like_as_nfs/repository.git'
    checkout_dir: .
```

ブランチ名指定でcheckoutします

```yaml
- uses: jksy/checkout-with-reference@v1
  with:
    branch_name: 'main'
    reference_dir: '/mnt/shared_dir_like_as_nfs/repository.git'
    checkout_dir: .
```
