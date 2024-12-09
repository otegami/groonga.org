# Copyright (C) 2024  Kodama Takuya <otegami@clear-code.com>
# Copyright (C) 2024  Horimoto Yasuhiro <horimoto@clear-code.com>
#
# This library is free software; you can redistribute it and/or
# modify it under the terms of the GNU Lesser General Public
# License as published by the Free Software Foundation; either
# version 2.1 of the License, or (at your option) any later version.
#
# This library is distributed in the hope that it will be useful,
# but WITHOUT ANY WARRANTY; without even the implied warranty of
# MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE. See the GNU
# Lesser General Public License for more details.
#
# You should have received a copy of the GNU Lesser General Public
# License along with this library; if not, write to the Free Software
# Foundation, Inc., 51 Franklin Street, Fifth Floor, Boston, MA 02110-1301 USA

require_relative "release_task"

release_task = ReleaseTask.new("Groonga", __dir__)
release_task.define

namespace :pgroonga do
  namespace :release do
    namespace :blog do
      desc "Generate release announce posts"
      task :generate do
        client = ReleaseTask::GitHubClient.new("pgroonga", "pgroonga")
        latest_release = client.latest_release
        # "PGroonga 3.2.4: 2024-10-03"
        release_name = latest_release["name"]
        # "3.2.4"
        version = release_name[/\d+\.\d+\.\d+/, 0]
        # "2024-10-03"
        release_date = Date.parse(release_name[/\d+-\d+-\d+/, 0])

        post_md = "#{release_date.strftime("%F")}-pgroonga-#{version}.md"
        ["ja", "en"].each do |locale|
          base_url = "https://pgroonga.github.io"
          product_label = "PGroonga"
          news_path = "/news/\#version-#{version.gsub(".", "-")}"
          case locale
          when "ja"
            base_url += "/ja"
            product_full_label =
              "PostgreSQL用ゼロETL高速日本語全文検索モジュール" +
              "PGroonga（ぴーじーるんが）"
            content = <<-CONTENT
---
layout: post.ja
title: #{product_full_label} #{version}リリース
description: #{product_full_label} #{version}をリリースしました！
---

## #{product_full_label} #{version}リリース

#{product_label} #{version}をリリースしました！

変更内容は[リリースノート](#{base_url}#{news_path})を参照してください。

PGroongaについては[PGroonga 3.0.0のリリースアナウンス]({% post_url #{locale}/2023-04-13-pgroonga-3.0.0 %})を参照してください。

開発者がGroonga・PGroonga・Mroongaのリリース内容について自慢する動画配信、[Groonga リリース自慢会](https://www.youtube.com/playlist?list=PLLwHraQ4jf7PnA3GjI9v90DZq8ikLk0iN)もあわせてご覧ください。

### インストール方法

まだインストールしていない場合は[インストール](#{base_url}/install/)を参考にインストールして、まずは[チュートリアル](#{base_url}/tutorial/)をやってみてください。

### アップグレード方法

2.0.0以降を使っている場合は[アップグレード](#{base_url}/upgrade/#compatible-case)の「互換性がある場合」用の手順でアップグレードしてください。

1.Y.Zを使っている場合は[アップグレード](#{base_url}/upgrade/#incompatible-case)の「非互換の場合」用の手順でアップグレードしてください。PGroonga 1系と3系は互換性が無いためです。

### サポートサービス

[PGroongaのサポートサービス](#{base_url}/support/)を提供しています。インデックスや検索の設計方法に関するコンサルティングやトラブル時の調査、パフォーマンス改善・新機能追加などの技術支援など、PGroongaに関わるサポートが必要な場合はご相談ください。

### まとめ

PostgreSQLで高速に日本語全文検索をしたいという方はPGroongaを使ってガンガン検索してください！
            CONTENT
          else
            product_full_label = "PGroonga (zero ETL fast full text search module for PostgreSQL)"
            content = <<-CONTENT
---
layout: post.en
title: #{product_full_label} #{version} has been released
description: #{product_full_label} #{version} has been released!
---

## #{product_full_label} #{version} has been released

#{product_label} #{version} has been released!

See [the release note](#{base_url}#{news_path}) for this release.

See [the PGroonga 3.0.0 release announce]({% post_url #{locale}/2023-04-13-pgroonga-3.0.0 %}) about PGroonga.

### How to install

If you haven't installed PGroonga yet, [install PGroonga](#{base_url}/install/) and try [tutorial](#{base_url}/tutorial/)!

### How to upgrade

If you're using PGroonga 2.0.0 or later, you can upgrade by steps in "Compatible case" in [the Upgrade document](#{base_url}/upgrade/#compatible-case).

If you're using PGroonga 1.Y.Z, you can upgrade by steps in "Incompatible case" in [the Upgrade document](#{base_url}/upgrade/#incompatible-case).

### Support service

If you need commercial support for PGroonga, [contact us](mailto:info@clear-code.com).

### Conclusion

Try PGroonga when you want to perform zero ETL fast full text search against all languages on PostgreSQL!
            CONTENT
          end
          File.open("#{locale}/_posts/#{post_md}", "w") do |post|
            post.write(content)
          end
        end
      end
    end
  end
end
