will_paginate 적용시에 겪었던 문제점

join + order + limit


이전 코드
def show
	@sub_menus = SubMenu.joins(:menu).includes(:items).where(category_id: session[:category_id]).order('menus.name collate "C" asc').order('items.nmc desc').limit(10)
end

@sub_menus.length
=> 6

SQL
  SQL (1.5ms)  SELECT  DISTINCT "sub_menus"."id", items.nmc AS alias_0 FROM "sub_menus" INNER JOIN "menus" ON "menus"."id" = "sub_menus"."menu_id" LEFT OUTER JOIN "items" ON "items"."sub_menu_id" = "sub_menus"."id" WHERE (menus.category_id = 1)  ORDER BY items.nmc desc LIMIT 10
  SQL (1.3ms)  SELECT "sub_menus"."id" AS t0_r0, "sub_menus"."name" AS t0_r1, "sub_menus"."created_at" AS t0_r2, "sub_menus"."updated_at" AS t0_r3, "sub_menus"."menu_id" AS t0_r4, "sub_menus"."description" AS t0_r5, "items"."id" AS t1_r0, "items"."name" AS t1_r1, "items"."description" AS t1_r2, "items"."url" AS t1_r3, "items"."created_at" AS t1_r4, "items"."updated_at" AS t1_r5, "items"."sub_menu_id" AS t1_r6, "items"."brand" AS t1_r7, "items"."comment" AS t1_r8, "items"."nmc" AS t1_r9, "items"."npc" AS t1_r10, "items"."log_updated_at" AS t1_r11 FROM "sub_menus" INNER JOIN "menus" ON "menus"."id" = "sub_menus"."menu_id" LEFT OUTER JOIN "items" ON "items"."sub_menu_id" = "sub_menus"."id" WHERE (menus.category_id = 1) AND "sub_menus"."id" IN (6, 1, 1, 6, 1, 31, 46, 51, 50, 6)  ORDER BY items.nmc desc


바뀐 코드
def show
	@sub_menus = SubMenu.joins(:menu).includes(:items).where("menus.category_id = 1").order('menus.name collate "C" asc').paginate(page: params[:page], per_page: 10)
end

<sub_menu.rb>
class SubMenu < ActiveRecord::Base
  has_many :items, -> { order(nmc: :desc) }, dependent: :destroy <== default ordering 추가
