# **Decrypt HTTPS Traffic in Wireshark On Ubuntu**

Being able to inspect HTTPS traffic in Wireshark is extremely useful when you're debugging APIs, analysing website behaviour, or learning how network protocols work behind the scenes. This guide walks through how to decrypt HTTPS traffic in Wireshark on Ubuntu using the `SSLKEYLOGFILE` method, no need for certificates or proxy setups.

---

## Part 1: Setting Up the `SSLKEYLOGFILE` Environment Variable

First, make sure Wireshark is installed on your Ubuntu system. You can install it with:

```bash
sudo apt install wireshark
```

Next, we need to set the `SSLKEYLOGFILE` environment variable. This allows supported browsers to log TLS keys to a file, which Wireshark can then use to decrypt HTTPS traffic.

Open your `.bashrc` file:

![Figure 2](../img/decrypt-wireshark-ubuntu/fig2.png)

```bash
nano ~/.bashrc
```

Scroll to the bottom and add:

```bash
export SSLKEYLOGFILE=~/.ssl-key.log
```

![Figure 1](../img/decrypt-wireshark-ubuntu/fig1.png)

Save the file by pressing `CTRL + O`, then exit with `CTRL + X`.

To apply the change, either close and reopen the terminal, or run:

```bash
source ~/.bashrc
```

You can confirm it's been set by running:

```bash
echo $SSLKEYLOGFILE
```

You should see the correct file path printed.

![Figure 3](../img/decrypt-wireshark-ubuntu/fig3.png)

---

## Part 2: Configuring Your Browser to Log SSL Keys

Now we need a browser that supports TLS key logging. Note that **Firefox on Linux no longer supports this method by default**, so you'll need to use **Chromium or Google Chrome**.

Important: If you're using Chromium or Chrome from **Snap** or **Flatpak**, this likely won’t work, they are sandboxed and cannot write to external files like `~/.ssl-key.log`.

To avoid this, install the `.deb` version of Chrome:

```bash
wget https://dl.google.com/linux/direct/google-chrome-stable_current_amd64.deb
sudo apt install ./google-chrome-stable_current_amd64.deb
```

Once installed, you need to launch Chrome from a terminal so it inherits the environment variable:

```bash
google-chrome &
```

![Figure 4](../img/decrypt-wireshark-ubuntu/fig4.png)

If you try to use Chromium installed via Snap, you'll likely see an error stating that it couldn't open the SSL key log file.

![Figure 5](../img/decrypt-wireshark-ubuntu/fig5.png)

When Chrome is working correctly, visiting a few HTTPS websites will cause the `.ssl-key.log` file to populate. You can check this by running:

```bash
cat ~/.ssl-key.log
```

Or by opening the file in a text editor. You should see lines like this:

![Figure 6](../img/decrypt-wireshark-ubuntu/fig6.png)

---

## Part 3: Configuring Wireshark to Use the SSL Key Log File

Now we’ll configure Wireshark to use the SSL key log file.

1. Open Wireshark
2. Go to `Edit` → `Preferences`
3. Navigate to `Protocols` → `TLS`
4. Set the **(Pre)-Master-Secret log filename** to (make sure to change to your username):

```bash
/home/breezy/.ssl-key.log
```

If you're using the file browser to locate it, make sure to enable "Show hidden files" so `.ssl-key.log` is visible.

![Figure 7](../img/decrypt-wireshark-ubuntu/fig7.png)

![Figure 8](../img/decrypt-wireshark-ubuntu/fig8.png)

Click **OK** to save the changes.

---

## Part 4: Capturing and Analysing HTTPS Traffic

Now you can begin capturing traffic.

1. Start a capture in Wireshark on your active interface (e.g. `wlan0`, `eth0`)
2. Launch the browser from your terminal:

    ```bash
    google-chrome &
    ```

3. Visit any HTTPS website

If everything is working, Wireshark will automatically decrypt the traffic using the key log file. You should start seeing decrypted HTTPS content.

![Figure 9](../img/decrypt-wireshark-ubuntu/fig9.png)

To filter just the decrypted traffic, use the following display filter:

```bash
http2
```

This will show you all HTTP/2 traffic, which is commonly used by modern websites. Here there is some entries for reCAPTCHA and Reddit, demonstrating that the HTTPS traffic is being captured and decrypted successfully.

![Figure 10](../img/decrypt-wireshark-ubuntu/fig10.png)

---

## Conclusion

With this method, you can easily inspect HTTPS traffic in Wireshark without needing a proxy or modifying certificate stores. It’s ideal for debugging browser-based network activity or exploring HTTP/2 in detail.

A few final notes:

* You must launch your browser from a terminal that has the `SSLKEYLOGFILE` variable set
* Snap and Flatpak versions of Chromium and Firefox won’t work with this method
* Google Chrome (installed via `.deb`) works most reliably