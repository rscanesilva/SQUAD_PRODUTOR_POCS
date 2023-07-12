<template>
  <div class="hello">
    <button @click="convertToPdf">Converter para PDF</button>
  </div>
</template>

<script>
import html2pdf from 'html2pdf.js'
import axios from 'axios'

export default {
  methods: {
    async convertToPdf() {
      const url = 'http://localhost:8080/verboleto.php?numero=20001857&banco=594295466&cpfcnpj=00005064143451' // Substitua pela URL da página HTML externa que você deseja converter

      try {
        axios.defaults.headers.common['Access-Control-Allow-Origin'] = '*';
        const response = await axios.get(url)
        const html = response.data
        console.log(html);
        const element = document.createElement('div')
        element.innerHTML = html
        
        const options = {
          margin: 10,
          filename: 'pagina.pdf',
          image: { type: 'jpeg', quality: 0.98 },
          html2canvas: { scale: 2 },
          jsPDF: { unit: 'mm', format: 'a3', orientation: 'portrait' }
        }

        await html2pdf().set(options).from(element).save()
      } catch (error) {
        console.error(error)
      }
    }
  }
}
</script>
