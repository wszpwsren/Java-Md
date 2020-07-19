# docker

//

docker run -di --network=host --name dfstraker -v /var/fdfs/tracker:/fastdfs/tracker/data season/fastdfs tracker

//

docker run -di --network=host --name dfsstorage -v /var/fdfs/storage:/fastdfs/storage/data -v /var/fdfs_path:/fastdfs/store_path -e TRACKER_SERVER:106.14.189.65:22122 season/fastdfs storage

## 新

docker run -di --network=host --name dfstracker -v /var/fdfs/tracker:/var/fdfs -v /etc/localtime:/etc/localtime delron/fastdfs tracker

docker run -di --network=host --name dfsstorage -e TRACKER_SERVER:106.14.189.65:22122 -v /var/fdfs/storage:/var/fdfs  -v /etc/localtime:/etc/localtime delron/fastdfs storage