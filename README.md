## Backend para upload de imagens

### Descrição
Backend construindo em NodeJS e Express utilizando o banco de dados MongoDB, os uploads podem ser feitos em disco local ou na nuvem basta ajustar as configurações do ```STORAGE_TYPE``` para "*s3*" de acordo com o arquivo ```multer.js```, e as demais variáveis no arquivo ```.env``` que também deverão ser configurado de acordo com a sua nuvem em AWS S3.
O bucket vazio em ```multer.js``` deverá ser alterado de acordo com o seu bucket, veja abaixo:

```js
const storageTypes = {

    /* .... */

    s3: multerS3({
        s3: new aws.S3(),
        bucket: '', // bucket a definir
        contentType: multerS3.AUTO_CONTENT_TYPE,
        acl: 'public-read',
        key: (req, file, callback) => {
            crypto.randomBytes(16, (err, hash) => {
                if (err) callback(err)

                const fileName = `${hash.toString('hex')}-${file.originalname}`

                callback(null, fileName)
            })
        }
    })
}
```