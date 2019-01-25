.PHONY: run scrape clean

run:
	docker-compose up

build:
	docker-compose build

scrape:
	docker-compose run --rm brozzler brozzler-new-site ${SEED} --ignore-robots

clean:
	docker-compose down
	docker volume rm brozzler_rethinkdb_data
