idf_build_get_property(project_dir PROJECT_DIR)
# Embed the server root certificate into the final binary
idf_component_register(SRCS "main.c" "wifi.c"
                    INCLUDE_DIRS "."
                    EMBED_TXTFILES ${project_dir}/server_certs/ca_cert.pem)

