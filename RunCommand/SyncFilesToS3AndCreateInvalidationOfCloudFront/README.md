# Sync Files To S3 And Create Invalidation Of CloudFront

## Overview

EC2インスタンス内の所定のパス（ディレクトリ）の内容をS3バケット上に`s3 sync`し、任意でCloudFrontのCreate Invalidation（キャッシュクリア）を実行するRunCommand用のSSMドキュメントです。

`--exact-timestamps`オプションを使って、`s3 sync`が行われるので、ファイルに変更がないときは、同期処理が行われないようになっています。

また、同期処理が行われなかったときは、CloudFrontのCreate Invalidationも行われないようになっています。

## Prerequisites

Amazon Linux 2での実行を前提として作成しており、コマンド実行するEC2インスタンスのインスタンスプロファイルにてaws cliの`s3 sync`、`cloudfront create-invalidation`が許可された権限が必要です。

## Use case

例えば、EC2インスタンス上で生成された静的ウェブサイトのホスティングをS3とCloudFrontで行っている時、
メンテナンスウインドウなどで定期実行するようにすれば、生成されたファイルをS3上にコピーするのと、CloudFrontのキャッシュクリアも自動で行われるので便利です。
