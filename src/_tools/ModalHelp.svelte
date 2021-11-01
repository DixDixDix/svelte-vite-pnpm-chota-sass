<script>
    import {Modal,Card,Button} from 'svelte-chota';
    export let url="";          // Адрес помощи
    export let open = false;    // Открытое окно или закрытое
    let html = "";
    // Перехватывает ссылки в модальном окне
    function innerLink(e) {
        let target = e.target.closest('a');
        let link = target.getAttribute('href');
        console.log("Link:", link, link.indexOf('://'));
        if (link.indexOf('://')<1) {
            let aUrl = new URL(url);
            link=aUrl.protocol+'//'+aUrl.host+link;
            url = link;
            console.log("Host is:", link, aUrl);
        }
        // Если был внутренний переход, у него может быть локальная ссылка /about
        // Поэтому, если нет протокола :// - нужно взять из предыдущего значения url
    }

    $: if (url) {
        const options = {
            mode: "cors"
        }
        console.log('Помощь - запрашиваю: ',url);
        fetch(`${url}?ajax`, options)
        .then((response) => {
            if (!response.ok) {
                throw Error(response.statusText);
            }
            return response.text()
        })
        .then((data) => {
            html=data;
            console.log('data:',data)
        })
        .catch((error) => {
            html=error;
            console.log('error:',error);
        })

}        

</script>

<Modal bind:open={open}>
    <Card style="max-width: 600px;max-height: 400px; overflow: auto;">
        <div on:click|preventDefault={innerLink}>{@html html}</div>
        <!--     <iframe sandbox title="help" loading = "lazy" src={url} height="500px" width="500px" style="border: none;">
            <p>Загружаю данные</p>
        </iframe> -->
        <div slot="footer" class="is-right">
            <Button clear on:click={()=>open=false}>Закрыть</Button>
        </div>
    </Card>
</Modal>