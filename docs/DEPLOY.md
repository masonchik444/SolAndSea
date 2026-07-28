# Публикация сайта на doremisolandsea.com

Сайт — статическая страница из папки `public/`. Хостинг: **Cloudflare Pages**
(бесплатно, работает с приватным репозиторием, обновляется сам при каждом
пуше). Домен зарегистрирован в **Namecheap** и сейчас смотрит на Tilda.

---

## Шаг 1. Создать проект в Cloudflare

В новом дашборде Cloudflare импорт репозитория ведёт в мастер **Workers**
(экран «Set up your application», команда `npx wrangler deploy`). Он рабочий —
в корне репозитория лежит `wrangler.toml`, который говорит Cloudflare
раздавать папку `public/` как статический сайт.

**Вариант А — Workers (тот экран, что открывается по умолчанию)**

1. Зарегистрироваться на [dash.cloudflare.com](https://dash.cloudflare.com/).
2. Импортировать репозиторий `masonchik444/SolAndSea`, авторизовав GitHub.
3. Поля оставить как есть:

   | Поле | Значение |
   |---|---|
   | Project name | `solandsea` (совпадает с `name` в `wrangler.toml`) |
   | Build command | *пусто* |
   | Deploy command | `npx wrangler deploy` |

4. **Deploy**. Сайт поднимется на адресе вида `solandsea.<ваш>.workers.dev`.

**Вариант Б — классический Pages**

Если хочется привычного интерфейса Pages: **Workers & Pages** → **Create** →
вкладка **Pages** → **Connect to Git**. Настройки: framework preset —
**None**, build command — пусто, build output directory — **`public`**.
`wrangler.toml` в этом случае просто не используется.

В обоих вариантах production-веткой нужно выбрать основную ветку репозитория
(`claude/b2b-tours-tilda-site-hpyn27`) и проверить сайт на временном адресе
до переключения домена.

## Шаг 2. Добавить домен в Cloudflare

Cloudflare Pages требует, чтобы домен обслуживался DNS-серверами Cloudflare.

1. В дашборде: **Add a site** → ввести `doremisolandsea.com` → выбрать план
   **Free**.
2. Cloudflare просканирует текущие записи и покажет **два своих
   nameserver-адреса**. Для этого домена выданы:

   ```
   laylah.ns.cloudflare.com
   rick.ns.cloudflare.com
   ```
3. Привязать домен к проекту:
   - в варианте Workers: проект → **Settings** → **Domains & Routes** →
     **Add** → **Custom domain** → `doremisolandsea.com`, затем так же
     `www.doremisolandsea.com`;
   - в варианте Pages: проект → **Custom domains** → **Set up a domain**.

## Шаг 3. Переключить домен в Namecheap

1. Войти в Namecheap → **Domain List** → напротив `doremisolandsea.com`
   нажать **Manage**. Нужна первая вкладка **Domain**, не Advanced DNS.
2. Секция **Nameservers**: в выпадающем списке вместо `Namecheap BasicDNS`
   выбрать **Custom DNS**.
3. В два появившихся поля вписать адреса Cloudflare, по одному в каждое:
   `laylah.ns.cloudflare.com` и `rick.ns.cloudflare.com`. Старые
   `dns1.registrar-servers.com` и `dns2.registrar-servers.com` при этом
   пропадают сами — отдельно удалять не нужно.
4. Сохранить зелёной галочкой справа.
5. Вернуться в Cloudflare и нажать **Continue** / **Check nameservers now**.

После этого DNS-зоной управляет Cloudflare, а старые записи Tilda
(A-запись на IP Тильды и CNAME для `www`) перестают действовать. Если
Cloudflare при сканировании перенёс их к себе — удалить их в разделе
**DNS → Records**, иначе домен продолжит открывать пробник Tilda.

Обновление занимает от 15 минут до 24 часов. HTTPS-сертификат Cloudflare
выпустит сам, ничего покупать не нужно.

## Шаг 4. Отключить домен в Tilda

В проекте Tilda: **Настройки сайта → Домен** — убрать `doremisolandsea.com`,
чтобы не было конфликта и чтобы Tilda не пыталась выпускать свой
сертификат.

---

## Как обновлять сайт дальше

Любой коммит в основную ветку → Cloudflare сам пересобирает и публикует за
20–40 секунд. Откат делается кнопкой **Rollback** в списке деплоев.

## Что нужно доделать до запуска

- [ ] Заменить три фотографии: `exc2.jpg` (Севилья), `hotel1.jpg` (Стамбул),
      `atr1.jpg` (горный треккинг) — см. `docs/PHOTOS.md`
- [x] Юридические страницы: `aviso-legal.html`, `privacidad.html`,
      `cookies.html` — на трёх языках, юридическую силу имеет испанская
      версия. В форме обязательный чекбокс согласия со ссылкой на
      политику. Баннер cookies не нужен: сайт не ставит ни аналитических,
      ни рекламных куки. **Если подключите аналитику — баннер станет
      обязательным**, а политику cookie нужно будет обновить.
      Тексты составлены по структуре LSSI-CE и RGPD, но перед серьёзными
      переговорами стоит показать их гестору.
- [x] Форма заявки отправляет заявки на `doremisolsea@gmail.com` через
      [FormSubmit](https://formsubmit.co/) (без регистрации и без сервера).
      **Требуется однократная активация:** после первой отправки с сайта
      FormSubmit пришлёт на этот адрес письмо со ссылкой — по ней нужно
      перейти, иначе заявки не доходят. Проверить попадание в «Спам».
      После активации в личном кабинете FormSubmit можно получить
      «хешированный» адрес формы и подставить его в `public/index.html`
      вместо открытой почты, чтобы её не собирали спам-боты.
- [x] Данные владельца в подвале: Yaroslav Tyutyunikov · Autónomo ·
      NIE Z1513422G · IAE 844 · Calle Escritor Ferrándiz Torremocha, 14 ·
      03011 Alicante
- [ ] Аналитика (Cloudflare Web Analytics включается в один клик и не
      требует баннера cookies)
- [ ] Загрузить логотип Jazz House в `public/assets/partners/jazz-house.png`
      (пока вместо него выводится название текстом)
- [ ] Проверить права на фотографии: снимки из интернета могут быть
      защищены авторским правом. Безопасные источники — Unsplash, Pexels
      или собственные кадры.

## Запасной вариант: Tilda

Если позже захотите редактировать сайт мышкой, гайд по сборке в Tilda лежит
в `docs/TILDA-GUIDE.md`. Учтите: свой домен на бесплатном тарифе Tilda не
подключается, нужен минимум Personal.
