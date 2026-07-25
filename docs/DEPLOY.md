# Публикация сайта на doremisolandsea.com

Сайт — статическая страница из папки `public/`. Хостинг: **Cloudflare Pages**
(бесплатно, работает с приватным репозиторием, обновляется сам при каждом
пуше). Домен зарегистрирован в **Namecheap** и сейчас смотрит на Tilda.

---

## Шаг 1. Создать проект в Cloudflare Pages

1. Зарегистрироваться на [dash.cloudflare.com](https://dash.cloudflare.com/)
   (если аккаунта ещё нет).
2. В меню слева: **Workers & Pages** → **Create** → вкладка **Pages** →
   **Connect to Git**.
3. Авторизовать GitHub и разрешить доступ к репозиторию
   `masonchik444/SolAndSea`.
4. Выбрать репозиторий, дальше настройки сборки:

   | Поле | Значение |
   |---|---|
   | Production branch | `claude/b2b-tours-tilda-site-hpyn27` (основная ветка) |
   | Framework preset | **None** |
   | Build command | *оставить пустым* |
   | Build output directory | `public` |

5. **Save and Deploy**. Через минуту сайт будет доступен по временному
   адресу вида `solandsea.pages.dev` — на нём стоит всё проверить до
   переключения домена.

## Шаг 2. Добавить домен в Cloudflare

Cloudflare Pages требует, чтобы домен обслуживался DNS-серверами Cloudflare.

1. В дашборде: **Add a site** → ввести `doremisolandsea.com` → выбрать план
   **Free**.
2. Cloudflare просканирует текущие записи и покажет **два своих
   nameserver-адреса** вида `xxx.ns.cloudflare.com` — их нужно записать.
3. В проекте Pages: **Custom domains** → **Set up a domain** →
   `doremisolandsea.com`. Повторить для `www.doremisolandsea.com`.

## Шаг 3. Переключить домен в Namecheap

1. Войти в Namecheap → **Domain List** → напротив `doremisolandsea.com`
   нажать **Manage**.
2. Раздел **Nameservers**: сменить текущее значение на **Custom DNS** и
   вписать два адреса из шага 2.
3. Сохранить (галочка справа).

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
- [ ] Страницы **Aviso legal** и **Política de privacidad** + баннер cookies:
      сайт собирает персональные данные через форму, в Испании это
      обязательный минимум (LSSI-CE и GDPR)
- [ ] Форма заявки сейчас ничего не отправляет — подключить сервис
      (Formspree, Web3Forms или Cloudflare Worker) на
      `doremisolandsea@gmail.com`
- [ ] Юридические данные компании в подвале (razón social, CIF)
- [ ] Аналитика (Cloudflare Web Analytics включается в один клик и не
      требует баннера cookies)

## Запасной вариант: Tilda

Если позже захотите редактировать сайт мышкой, гайд по сборке в Tilda лежит
в `docs/TILDA-GUIDE.md`. Учтите: свой домен на бесплатном тарифе Tilda не
подключается, нужен минимум Personal.
