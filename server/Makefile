SERVER = uchat_server

EMPTY = 
SPACE = $(EMPTY) $(EMPTY)
VPATH = VPATH = $(subst $(SPACE), :, $(SRCD))

OBJD = obj
INCD_LIBS = -I../libraries/include
LIBD_LIBS = -L../libraries/lib
INCD_SERVER = -Iinc/

SRCD = src src/main src/json_objects src/database
INCD = inc
SRCS = $(foreach dir, $(SRCD), $(wildcard $(dir)/*.c))
OBJS = $(addprefix $(OBJD)/, $(notdir $(SRCS:%.c=%.o)))
INCS = $(foreach dir, $(INCD), $(wildcard $(dir)/*.h))

CC = clang

CFLAGS = -std=c11 $(addprefix -W, error no-deprecated all extra pedantic)
CPPFLAGS = -DMX_SERVER='"$(SERVER_WD)"' $(UTILS_INC) \
$(INCD_SERVER) -D_GNU_SOURCE $(INCD_LIBS) -I/usr/local/opt/openssl/include
LIBS = $(LIBD_LIBS) -L/usr/local/opt/openssl/lib \
-lcjson -lsqlite3 -lpthread -ldl

all: $(SERVER)

$(SERVER): $(OBJS)
	@$(CC) -fsanitize=address -g -o $@ $^ $(UTILS_LIB) $(LIBS)
	@printf "\033[32;1m$@ created\033[0m\n"

$(OBJS): obj/%.o: %.c $(INCS) | $(OBJD)
	@$(CC) $(CPPFLAGS) $(CFLAGS) -g -c -o $@ $<
	@printf "\033[32mcompiled: \033[0m$(notdir $<)\n"

$(OBJD):
	@mkdir -p $@
	@printf "\033[32;1m$@ created\033[0m\n"

uninstall: clean
	@rm -rf $(SERVER)
	@printf "\033[34;1mdeleted $(SERVER)\033[0m\n"

clean:
	@rm -rf $(OBJD)
	@rm -rf $(SERVER_WD)/files
	@rm -rf $(SERVER_WD)/database.db
	@printf "\033[34;1mdeleted $(OBJD)\033[0m\n"

reinstall: uninstall all

.PHONY: all uninstall clean reinstall