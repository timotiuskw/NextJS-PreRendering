# Pre-rendering di Next.js

## Pengertian Pre-rendering
Pre-rendering adalah proses membuat halaman web menjadi HTML statis terlebih dahulu saat aplikasi dibangun. Dengan pre-rendering, halaman bisa langsung ditampilkan ke pengguna tanpa perlu diproses ulang di server setiap kali ada yang membuka halaman tersebut.

Next.js mendukung dua jenis pre-rendering:
1. **Static Generation (SSG)**: Membuat halaman statis saat build.
2. **Server-side Rendering (SSR)**: Halaman di-render di server pada setiap permintaan.

Pre-rendering mengoptimalkan kinerja aplikasi dengan memberikan waktu muat yang lebih cepat kepada pengguna.

## Jenis Pre-rendering di Next.js

### 1. Static Generation (SSG)
Static Generation adalah proses di mana Next.js membuat HTML statis untuk setiap halaman pada saat build. Halaman ini tidak bergantung pada data dari server saat rendering.

![Static-Generation.png](https://nextjs.org/static/images/learn/data-fetching/static-generation.png)

### Penjelasan Static Generation

- Pada *Static Generation*, HTML untuk halaman web di-*generate* saat *build-time*, yaitu ketika aplikasi dibangun untuk produksi.
- Setelah proses *build* selesai, HTML yang sudah di-*generate* tersebut akan disimpan dan dapat langsung digunakan kembali untuk setiap permintaan (*request*) pengguna.
- Karena HTML ini sudah ada sebelum adanya permintaan, Next.js tidak perlu memproses ulang untuk menghasilkan HTML tersebut setiap kali pengguna mengakses halaman. Hal ini membuat proses menjadi lebih cepat dan efisien, terutama untuk konten yang jarang berubah.


Contoh penggunaan Static Generation dengan `getStaticProps`:

```js
export async function getStaticProps() {
  const data = await fetch('https://api.example.com/data');
  const json = await data.json();
  return {
    props: {
      data: json,
    },
  };
}

const HomePage = ({ data }) => {
  return (
    <div>
      <h1>Data</h1>
      <pre>{JSON.stringify(data, null, 2)}</pre>
    </div>
  );
};

export default HomePage;
```

Pada contoh di atas, `getStaticProps` di-execute saat build untuk mengambil data yang diperlukan dan memberikan data tersebut sebagai props ke komponen halaman. Halaman ini akan dirender menjadi HTML statis.

### 2. Server-side Rendering (SSR)

Server-side Rendering adalah proses di mana HTML halaman di-render di server pada setiap permintaan. SSR sangat cocok ketika data halaman sering berubah atau tergantung pada data dinamis yang tidak bisa diprediksi saat build.

![Server-side-rendering.png](https://nextjs.org/static/images/learn/data-fetching/server-side-rendering.png)

#### Penjelasan Server-side Rendering (SSR)

- Pada *Server-side Rendering*, HTML untuk setiap halaman web di-*generate* di server saat permintaan (request) dilakukan oleh pengguna.
- Setiap kali pengguna meminta halaman, server akan membangkitkan HTML secara dinamis berdasarkan permintaan tersebut.
- Setelah HTML di-*generate*, server mengirimkan HTML ke pengguna, dan proses ini diulang untuk setiap permintaan yang diterima.


Contoh penggunaan SSR dengan `getServerSideProps`:

```js
export async function getServerSideProps() {
  const data = await fetch('https://api.example.com/data');
  const json = await data.json();
  return {
    props: {
      data: json,
    },
  };
}

const HomePage = ({ data }) => {
  return (
    <div>
      <h1>Data</h1>
      <pre>{JSON.stringify(data, null, 2)}</pre>
    </div>
  );
};

export default HomePage;
```

Pada contoh ini, `getServerSideProps` akan dijalankan setiap kali halaman diminta. Data akan diambil di server pada saat permintaan terjadi, dan halaman akan dirender secara dinamis dengan data tersebut.