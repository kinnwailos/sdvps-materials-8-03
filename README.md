## Дополнительные материалы для выполнения домашних заданий из блока "Введение в DevOps"


- [Дополнительный материал для занятия "8.2. Что такое DevOps. СI/СD"](CICD/8.2-hw.md)

- [Дополнительный материал для занятия "8.3. GitLab"](https://github.com/netology-code/sdvps-materials/tree/main/gitlab)

# Домашнее задание: Что такое DevOps. CI/CD

**ФИО**: Захаров Иван Андреевич
**Дата**: 08.03.2026

---

## Задание 1: Jenkins Freestyle Project
- Установлен Jenkins, настроен Freestyle Project
- Выполняются: `go test` + `docker build`
![1](Pictures/Screenshot_2026-02-25_12-25-27.png)
![2](Pictures/Screenshot_2026-02-25_12-23-30.png)



## Задание 2: Declarative Pipeline
- Создан Jenkinsfile с declarative syntax
- Stage: Checkout → Test → Build Binary → Docker → Upload
![3](Pictures/Screenshot_2026-03-08_16-12-22.png)
![4](Pictures/Screenshot_2026-03-08_16-11-07.png)
![5](Pictures/Screenshot_2026-03-08_16-12-49.png)

## Задание 3: Nexus + Upload
- Установлен Nexus 3.89.0, создан raw-hosted репозиторий `go-binaries`
- Pipeline загружает бинарник через curl с авторизацией
![6](Pictures/Screenshot_2026-03-08_16-13-14.png)
![7](Pictures/Screenshot_2026-03-08_16-13-29.png)
![8](Pictures/Screenshot_2026-03-08_16-14-09.png)