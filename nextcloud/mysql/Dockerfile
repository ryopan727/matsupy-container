#使うDockerイメージ
FROM mysql:8.4

#ポートの設定
EXPOSE 3306

#MySQL設定ファイルをコピー
COPY ./my.cnf /etc/mysql/conf.d/my.cnf

#docker run時の実行コマンド
CMD ["mysqld"]