# WinSession — Windows Codespace Reconnect Website

Ye static website tumhare **pehle se bane** Windows 10 aur Windows 11 GitHub Codespaces ko start karne ke liye hai. Ye naya Codespace nahi banati.

## 1) Website chalana

1. `index.html` ko browser mein kholo. Best option: is folder ko kisi static host par publish karo (GitHub Pages / Cloudflare Pages / Netlify).
2. GitHub fine-grained personal access token banao: https://github.com/settings/personal-access-tokens/new
3. Token ko sirf website ke token box mein paste karo.
4. `Check both sessions` se status dekho. Machine ke `Start & open` button se usi purane Codespace ko start karo.

## 2) Token permissions

- Token type: Fine-grained personal access token.
- Resource owner: wahi GitHub account jisme Codespaces hain.
- Repository access: agar GitHub selection allow kare to `amffcomedy2-maker/windows` select karo.
- Repository permissions:
  - `Codespaces`: Read-only
  - `Codespaces lifecycle admin`: Read and write

Token ko GitHub repository mein kabhi commit mat karna. Website token ko localStorage/cookie mein store nahi karti; token browser tab ke memory/input mein rahta hai aur GitHub API par request karne ke liye use hota hai. Tab close karne par field clear ho jati hai. Baad mein token revoke kar sakte ho.

## 3) Original session aur data

Tumhare Codespace URL prefixes se ye identifiers liye gaye hain:

- Windows 10: `organic-bassoon-r7w55494pvr9hpqq5`
- Windows 11: `glorious-journey-5v577g6gxr593v7qr`

Website GitHub API ke through `POST /user/codespaces/{codespace_name}/start` chalati hai; GitHub docs ke mutabik is action ke liye `Codespaces lifecycle admin: write` permission chahiye.

Repo config mein Windows disk `/storage` par `./windows:/storage` bind mount se mapped hai. Isi wajah se Codespace workspace ke bache rehne par Windows ka disk data bhi bacha rehna chahiye. **Codespace delete mat karna**, aur `.devcontainer/windows` ya Windows storage folder ko manually delete mat karna.

GitHub Codespaces ka maximum lifetime **12 hours** hai. 12 ghante baad running machine stop ho sakti hai; ye website usi Codespace ko dobara start kar sakti hai. Windows phir boot hoga—RAM mein chal raha exact live app/process state restore nahi hota, lekin persistent disk par save ki hui files, installed apps aur Windows settings rehni chahiye.

Agar 240 hours (10 days) tak Codespace band rahe, GitHub Settings → Codespaces mein stopped Codespaces ka retention period kam-se-kam 10 days rakho. GitHub ka default retention 30 days hai; `Keep codespace` option se automatic deletion roki ja sakti hai. Storage allowance/charges apply ho sakte hain.

## 4) Troubleshooting

- `401`: token invalid/expired ya wrong account.
- `403`: token permission missing, billing/plan restriction, ya organization policy.
- `404`: Codespace name is GitHub account ke liye nahi mila, ya Codespace delete ho chuka hai.
- `402`: Codespaces billing/usage restriction.
- Port `8006` open na ho: GitHub Codespaces page par original Codespace status check karo; service start hone ke baad original link refresh karo.

Official docs:
- Lifecycle / 12-hour maximum: https://docs.github.com/en/codespaces/about-codespaces/understanding-the-codespace-lifecycle
- Codespace retention: https://docs.github.com/en/codespaces/setting-your-user-preferences/configuring-automatic-deletion-of-your-codespaces
- Codespaces REST API: https://docs.github.com/en/rest/codespaces/codespaces
