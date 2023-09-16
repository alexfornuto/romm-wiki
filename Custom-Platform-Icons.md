While RomM supports every platform listed in the [IGDB platforms page](https://www.igdb.com/platforms), only a portion of those platforms have custom icons built-in. If your library has platforms that aren't listed in [the platforms icons list](https://github.com/zurdi15/romm/tree/release/frontend/assets/platforms), RomM will display a default fallback icon.

If you'd like to load your own custom icons for missing platforms, you can mount `/var/www/html/assets/platforms` to some local folder and place all of your custom **`.ico`** platform icons in there. You'll also want to download the ones [provided in this project](https://github.com/zurdi15/romm/tree/release/frontend/assets/platforms) and place them in the same folder.

**The name of the `.ico` file should match the slug of the platform on IGDB.** For example, the URL for the AmstradCPC is https://www.igdb.com/platforms/__acpc__, so the filename should be `acpc.ico`.

<img width="2459" alt="Screenshot 2023-09-15 at 10 45 04 AM" src="https://github.com/zurdi15/romm/assets/3247106/1831c206-b431-41c2-9761-49c132f40ee0">